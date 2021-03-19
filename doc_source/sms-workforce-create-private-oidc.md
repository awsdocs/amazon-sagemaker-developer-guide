# Create a Private Workforce \(OIDC IdP\)<a name="sms-workforce-create-private-oidc"></a>

Create a private workforce using an OpenID Connect \(OIDC\) Identity Provider \(IdP\) when you want to authenticate and manage workers using your own identity provider\. Use this page to learn how to configure your IdP to communicate with Amazon SageMaker Ground Truth \(Ground Truth\) or Amazon Augmented AI \(Amazon A2I\) and to learn how to create a workforce using your own IdP\. 

To create a workforce using an OIDC IdP, your IdP must support *groups* because Ground Truth and Amazon A2I use one or more groups that you specify to create work teams\. You use work teams to specify workers for your labeling jobs and human review tasks\. Because groups are not a [standard claim](https://openid.net/specs/openid-connect-core-1_0.html#StandardClaims), your IdP may have a different naming convention for a group of users \(workers\)\. Therefore, you must identify one or more user groups to which a worker belongs using the custom claim `sagemaker:groups` that is sent to Ground Truth or Amazon A2I from your IdP\. To learn more, see [Send Required and Optional Claims to Ground Truth and Amazon A2I](#sms-workforce-create-private-oidc-configure-idp)\.

You create an OIDC IdP workforce using the SageMaker API operation [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkforce.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkforce.html)\. Once you create a private workforce, that workforce and all work teams and workers associated with it are available to use for all Ground Truth labeling job tasks and Amazon A2I human review workflows tasks\. To learn more, see [Create an OIDC IdP Workforce](#sms-workforce-create-private-oidc-createworkforce)\.

## Send Required and Optional Claims to Ground Truth and Amazon A2I<a name="sms-workforce-create-private-oidc-configure-idp"></a>

When you use your own IdP, Ground Truth and Amazon A2I use your `Issuer`, `ClientId`, and `ClientSecret` to authenticate workers by obtaining an authentication CODE from your `AuthorizationEndpoint`\. 

Ground Truth and Amazon A2I will use this CODE to obtain a custom claim from either your IdP's `TokenEndpoint` or `UserInfoEndpoint`\. You can either configure `TokenEndpoint` to return a JSON web token \(JWT\) or `UserInfoEndpoint` to return a JSON object\. The JWT or JSON object must contain required and optional claims that you specify\. A [https://openid.net/specs/openid-connect-core-1_0.html#Terminology](https://openid.net/specs/openid-connect-core-1_0.html#Terminology) is a key\-value pair that contains information about a worker or metadata about the OIDC service\. The following table lists the claims that must be included, and that can optionally be included in the JWT or JSON object that your IdP returns\. 

**Note**  
Some of the parameters in the following table can be specified using a `:` or a `-`\. For example, you can specify the groups a worker belongs to using `sagemaker:groups` or `sagemaker-groups` in your claim\. 


|  Name  | Required | Accepted Format and Values | Description | Example | 
| --- | --- | --- | --- | --- | 
|  `sagemaker:groups` or `sagemaker-groups`  |  Yes  |  **Data type**: If a worker belongs to a single group, identify the group using a string\. If a worker belongs to multiple groups, use a list of up to 10 strings\.  **Allowable characters**: Regex: \[\\p\{L\}\\p\{M\}\\p\{S\}\\p\{N\}\\p\{P\}\]\+ **Quotas**: 10 groups per worker 63 characters per group name  |  Assigns a worker to one or more groups\. Groups are used to map the worker into work teams\.   |  Example of worker that belongs to a single group: `"work_team1"` Example of a worker that belongs to more than one groups: `["work_team1", "work_team2"]`   | 
|  `sagemaker:sub` or `sagemaker-sub`  |  Yes  |  **Data type**: String  |  This is mandatory to track a worker identity inside the Ground Truth platform for auditing and to identify tasks worked on by that worker\.  For ADFS: Customers must use the Primary Security Identifier \(SID\)\.   |  `"111011101-123456789-3687056437-1111"`  | 
|  `sagemaker:client_id` or `sagemaker-client_id`  |  Yes  |  **Data type**: String **Allowable characters**: Regex: \[\\w\+\-\]\+ **Quotes**: 128 characters   |  A client ID\. All tokens must be issued for this client ID\.   |  `"00b600bb-1f00-05d0-bd00-00be00fbd0e0"`  | 
|  `sagemaker:name` or `sagemaker-name`  |  Yes  |  **Data type**: String  |  The worker name to be displayed in the worker portal\.  |  `"Jane Doe"`  | 
|  `email`  |  No  |  **Data type**: String  |  The worker email\. Ground Truth uses this email to notify workers that they have been invited to work on labeling tasks\. Ground Truth will also use this email to notify your workers when labeling tasks become available if you set up an Amazon SNS topic for a work team that this worker is on\.  |  `"example-email@domain.com"`  | 
|  `email_verified`  |  No  |  **Data type**: Bool **Accepted Values:** `True`, `False`  |  Indicates if the user email was verified or not\.   |  `True`  | 

The following an example of the JSON object syntax your `UserInfoEndpoint` can return\. 

```
{
    "sub":"122",
    "exp":"10000",
    "sagemaker-groups":["group1","group2"]
    "sagamaker-name":"name",
    "sagemkaer-sub":"122",
    "sagemaker-client_id":"123456"
}
```

Ground Truth or Amazon A2I compares the groups listed in `sagemaker:groups` or `sagemaker-groups` to verify that your worker belongs to the work team specified in the labeling job or human review task\. After the work team has been verified, labeling or human review tasks are sent to that worker\. 

## Create an OIDC IdP Workforce<a name="sms-workforce-create-private-oidc-createworkforce"></a>

You can create a workforce using the SageMaker API operation `CreateWorkforce` and associated language\-specific SDKs\. Specify a `WorkforceName` and information about your OIDC IDP in the parameter `OidcConfig`\. It is recommended that you configure your OIDC with a place\-holder redirect URI, and then update the URI with the worker portal URL after you create the workforce\. To learn more, see [Configure your OIDC IdP](#sms-workforce-create-private-oidc-configure-url)\.

The following shows an example of the request\. See [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkforce.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkforce.html) to learn more about each parameter in this request\.

```
CreateWorkforceRequest: {
    #required fields
    WorkforceName: "example-oidc-workforce",
    OidcConfig: { 
        ClientId: "clientId",
        ClientSecret: "secret",
        Issuer: "https://example-oidc-idp.com/adfs",
        AuthorizationEndpoint: "https://example-oidc-idp.com/adfs/oauth2/authorize",
        TokenEndpoint: "https://example-oidc-idp.com/adfs/oauth2/token",
        UserInfoEndpoint: "https://example-oidc-idp.com/adfs/oauth2/userInfo",
        LogoutEndpoint: "https://example-oidc-idp.com/adfs/oauth2/log-out",
        JwksUri: "https://example-oidc-idp.com/adfs/discovery/keys"
    },
    SourceIpConfig: {
        Cidrs: ["string", "string"]
    }
}
```

### Configure your OIDC IdP<a name="sms-workforce-create-private-oidc-configure-url"></a>

How you configure your OIDC IdP depends on the IdP you use, and your business requirements\. 

When you configure your IdP, you must to specify a callback or redirect URI\. After Ground Truth or Amazon A2I authenticates a worker, this URI will redirect the worker to the worker portal where the workers can access labeling or human review tasks\. To create a worker portal URL, you need to create a workforce with your OIDC IdP details using the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkforce.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkforce.html) API operation\. Specifically, you must configure your OIDC IdP with required custom sagemaker claims \(see the next section for more details\)\. Therefore, it is recommended that you configure your OIDC with a place\-holder redirect URI, and then update the URI after you create the workforce\. See [Create an OIDC IdP Workforce](#sms-workforce-create-private-oidc-createworkforce) to learn how to create a workforce using this API\. 

You can view your worker portal URL in the SageMaker Ground Truth console, or using the SageMaker API operation, `DescribeWorkforce`\. The worker portal URL is in the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Workforce.html#sagemaker-Type-Workforce-SubDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Workforce.html#sagemaker-Type-Workforce-SubDomain) parameter in the response\.

**Important**  
Make sure you add the workforce subdomain to your OIDC IdP allow list\. When you add the subdomain to your allow list, it must end with `/oauth2/idpresponse`\.

**To view your worker portal URL after creating a private workforce \(Console\):**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. In the navigation pane, choose **Labeling workforces**\. 

1. Select the **Private** tab\.

1. In **Private workforce summary** you will see **Labeling portal sign\-in URL**\. This is your worker portal URL\.

**To view your worker portal URL after creating a private workforce \(API\):**

When you create a private workforce using `[CreateWorkforce](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkforce.html)`, you specify a `WorkforceName`\. Use this name to call [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeWorkforce.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeWorkforce.html)\. The following table includes examples of requests using the AWS CLI and AWS SDK for Python \(Boto3\)\. 

------
#### [ SDK for Python \(Boto3\) ]

```
response = client.describe_workforce(WorkforceName='string')
print(f'The workforce subdomain is: {response['SubDomain']}')
```

------
#### [ AWS CLI ]

```
$ C:\>  describe-workforce --workforce-name 'string'
```

------

## Validate Your OIC IdP Workforce Authentication Response<a name="sms-workforce-create-private-oidc-validate"></a>

After you have created your OIDC IdP workforce, you can use the following procedure to validate its authentication workflow using cURL\. This procedure assumes you have access to a terminal, and that you have cURL installed\.

**To validate your OIDC IdP authorization response:**

1. Get an authorization code using a URI configured as follows:

   ```
   {AUTHORIZE ENDPOINT}?client_id={CLIENT ID}&redirect_uri={REDIRECT URI}&scope={SCOPE}&response_type=code
   ```

   1. Replace *`{AUTHORIZE ENDPOINT}`* with the authorize endpoint for your OIDC IdP\.

   1. Replace `{CLIENT ID}` with the Client ID from your OAuth client\.

   1. Replace *`{REDIRECT URI}`* with the worker portal URL\. If it is not already present, you must add `/oauth2/idpresponse` to the end of the URL\.

   1. If you have a custom scope, use it to replace `{SCOPE}`\. If you do not have a custom scope, replace `{SCOPE}` with `openid`\.

   The following is an example of a URI after the modifications above are made:

   ```
   https://example.com/authorize?client_id=f490a907-9bf1-4471-97aa-6bfd159f81ac&redirect_uri=https%3A%2F%2F%2Fexample.labeling.sagemaker.aws%2Foauth2%2Fidpresponse&response_type=code&scope=openid
   ```

1. Copy and paste the modified URI from step 1 into your browser and press Enter on your keyboard\.

1. Authenticate using your IdP\.

1. Copy the authentication code query parameter in the URI\. This parameter beings with `code=`\. The following is an example of what the response might look like\. In this example, copy `code=MCNYDB...` and everything thereafter\.

   ```
   https://example.labeling.sagemaker.aws/oauth2/idpresponse?code=MCNYDB....
   ```

1. Open a terminal and enter the following command after making required modifications listed below:

   ```
   $ curl --request POST \
     --url '{TOKEN ENDPOINT}' \
     --header 'content-type: application/x-www-form-urlencoded' \
     --data grant_type=authorization_code \
     --data 'client_id={CLIENT ID}' \
     --data client_secret={CLIENT SECRET} \
     --data code={CODE} \
     --data 'redirect_uri={REDIRECT URI}'
   ```

   1. Replace `{TOKEN ENDPOINT}` with the token endpoint for your OIDC IdP\.

   1. Replace `{CLIENT ID}` with the Client ID from your OAuth client\.

   1. Replace `{CLIENT SECRET}` with the Client Secret from your OAuth client\.

   1. Replace `{CODE}` with the authentication code query parameter you copied in step 4\.

   1. Replace *`{REDIRECT URI}`* with the worker portal URL\.

   The following is an example of the cURL request after making the modifications described above:

   ```
   $ curl --request POST \
     --url 'https://example.com/token' \
     --header 'content-type: application/x-www-form-urlencoded' \
     --data grant_type=authorization_code \
     --data 'client_id=f490a907-9bf1-4471-97aa-6bfd159f81ac' \
     --data client_secret=client-secret \
     --data code=MCNYDB... \
     --data 'redirect_uri=https://example.labeling.sagemaker.aws/oauth2/idpresponse'
   ```

1. This step depends on the type of `access_token` your IdP returns, a plain text access token or a JWT access token\.
   + If your IdP does not support JWT access tokens, `access_token` may be plain text \(for example, a UUID\)\. The response you see may look similar to the following\. In this case, move to step 7\.

     ```
     {
       "access_token":"179c144b-fccb-4d96-a28f-eea060f39c13",
       "token_type":"Bearer",
       "expires_in":3600,
       "refresh_token":"ef43e52e-9b4f-410c-8d4c-d5c5ee57631a",
       "scope":"openid"
     }
     ```
   + If your IdP supports JWT access tokens, step 5 should generate an access token in JWT format\. For example, the response may look similar to the following:

     ```
     {
         "access_token":"eyJh...JV_adQssw5c",
         "refresh_token":"i6mapTIAVSp2oJkgUnCACKKfZxt_H5MBLiqcybBBd04",
         "refresh_token_expires_in":6327,
         "scope":"openid",
         "id_token":"eyJ0eXAiOiJK9...-rDaQzUHl6cQQWNiDpWOl_lxXjQEvQ"
     }
     ```

     Copy the JWT and decode it\. You can use python script or a third party website to decode it\. For example, you can go to the website [https://jwt.io/](https://jwt.io/) and paste the JWT into the **Encoded** box to decode it\. 

     Make sure the decoded response contains the following:
     + The **Required** SageMaker claims in the table found in [Send Required and Optional Claims to Ground Truth and Amazon A2I](#sms-workforce-create-private-oidc-configure-idp)\. If it does not, you must reconfigure your OIDC IdP to contain these claims\. 
     + The [Issuer](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OidcConfig.html#sagemaker-Type-OidcConfig-Issuer) you specified when you set up the IdP workforce\.

1. In a terminal and enter the following command after making required modifications listed below:

   ```
   curl -X POST -H 'Authorization: Bearer {ACCESS TOKEN}' -d '' -k -v {USERINFO ENDPOINT}
   ```

   1. Replace `{USERINFO ENDPOINT}` with the user info endpoint for your OIDC IdP\.

   1. Replace `{ACCESS TOKEN}` with the access token in the response you received in step 7\. This is the entry for the `"access_token"` parameter\.

   The following is an example of the cURL request after making the modifications described above:

   ```
    curl -X POST -H 'Authorization: Bearer eyJ0eX...' -d '' -k -v https://example.com/userinfo
   ```

1. The response to the final step in the procedure above may look similar to the following code block\. 

   If the `access_token` returned in step 6 was plain text, you must verify that this response contains required information\. In this case, the response must contain the **Required** SageMaker claims in the table found in [Send Required and Optional Claims to Ground Truth and Amazon A2I](#sms-workforce-create-private-oidc-configure-idp)\. For example, `sagemaker-groups`, `sagamaker-name`\.

   ```
   {
       "sub":"122",
       "exp":"10000",
       "sagemaker-groups":["group1","group2"]
       "sagamaker-name":"name",
       "sagemkaer-sub":"122",
       "sagemaker-client_id":"123456"
   }
   ```

## Next Steps<a name="sms-workforce-create-private-oidc-next-steps"></a>

Once you've created a private workforce using your IdP and verified your IdP authentication response, you can create work teams using your IdP groups\. To learn more, see [Manage a Private Workforce \(OIDC IdP\)](sms-workforce-manage-private-oidc.md)\. 

You can restrict worker access to tasks to specific IP addresses, and update or delete your workforce using the SageMaker API\. To learn more, see [Manage Private Workforce Using the Amazon SageMaker API](sms-workforce-management-private-api.md)\.