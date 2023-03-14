# Connect to an Amazon EMR cluster<a name="scheduled-notebook-connect-emr"></a>

If you connect to an Amazon EMR cluster in your Jupyter notebook, you might need to perform additional setup\. In particular, the following discussion addresses two issues:
+ **Passing parameters into your Amazon EMR connection command**\. In SparkMagic kernels, parameters you pass to your Amazon EMR connection command may not work as expected due to differences in how Papermill passes parameters and how SparkMagic receives parameters\. The workaround to address this limitation is to pass parameters as environment variables\. For more details about the issue and workaround, see [Pass parameters to your EMR connection command](#scheduled-notebook-connect-emr-pass-param)\.
+ **Passing user credentials to Kerberos, LDAP, or HTTP Basic Auth\-authenticated Amazon EMR clusters**\. In interactive mode, Studio asks for credentials in a popup form where you can enter your sign\-in credentials\. In your non\-interactive scheduled notebook, you have to pass them through the AWS Secrets Manager\. For more details about how to use the AWS Secrets Manager in your scheduled notebook jobs, see [Pass user credentials to your Kerberos, LDAP, or HTTP Basic Auth\-authenticated Amazon EMR cluster](#scheduled-notebook-connect-emr-credentials)\.

## Pass parameters to your EMR connection command<a name="scheduled-notebook-connect-emr-pass-param"></a>

If you are using images with the SparkMagic PySpark and Spark kernels and want to parameterize your EMR connection command, provide your parameters in the **Environment variables** field instead of the Parameters field in the Create Job form \(in the **Additional Options** dropdown menu\)\. You also have to make sure your EMR connection command in the Jupyter notebook passes these parameters as environment variables\. For example, suppose you pass `cluster-id` as an environment variable when you create your job\. Your EMR connection command should look like the following:

```
%%local
import os
```

```
%sm_analytics emr connect —cluster-id {os.getenv('cluster_id')} --auth-type None
```

You need this workaround to meet requirements by SparkMagic and Papermill\. For background context, the SparkMagic kernel expects that the `%%local` magic command accompany any local variables you define\. However, Papermill does not pass the `%%local` magic command with your overrides\. In order to work around this Papermill limitation, you must supply your parameters as environment variables in the **Environment variables** field\.

## Pass user credentials to your Kerberos, LDAP, or HTTP Basic Auth\-authenticated Amazon EMR cluster<a name="scheduled-notebook-connect-emr-credentials"></a>

To establish a secure connection to an Amazon EMR cluster that uses Kerberos, LDAP, or HTTP Basic Auth authentication, you use the AWS Secrets Manager to pass user credentials to your connection command\. For information about how to create a Secrets Manager secret, see [Create an AWS Secrets Manager secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html)\. You pass the secret with the `--secrets` argument, as shown in the following example:

```
%sm_analytics emr connect --cluster-id j_abcde12345 
    --auth Kerberos 
    --secret aws_secret_id_123
```

Your administrator can set up a flexible access policy using an attribute\-based\-access\-control \(ABAC\) method, which assigns access based on special tags\. You can set up flexible access to create a single secret for all users in the account or a secret for each user\. The following code samples demonstrate these scenarios:

**Create a single secret for all users in the account**

```
{
    "Version" : "2012-10-17",
    "Statement" : [ 
        {
            "Effect": "Allow",
            "Principal" : {"AWS" : "arn:aws:iam::AWS_ACCOUNT_ID:role/service-role/AmazonSageMaker-ExecutionRole-20190101T012345"},

            "Action" : "secretsmanager:GetSecretValue",
            "Resource" : [  "arn:aws:secretsmanager:us-west-2:AWS_ACCOUNT_ID:secret:aes123-1a2b3c", 
                            "arn:aws:secretsmanager:us-west-2:AWS_ACCOUNT_ID:secret:aes456-4d5e6f", 
                            "arn:aws:secretsmanager:us-west-2:AWS_ACCOUNT_ID:secret:aes789-7g8h9i" ]
        }
    ]
}
```

**Create a different secret for each user**

You can create a different secret for each user using the `PrincipleTag` tag, as shown in the following example:

```
{
    "Version" : "2012-10-17",
    "Statement" : [ 
        {
            "Effect": "Allow",
            "Principal" : {"AWS" : "arn:aws:iam::AWS_ACCOUNT_ID:role/service-role/AmazonSageMaker-ExecutionRole-20190101T012345"},
            "Condition" : {
                "StringEquals" : {
                    "aws:ResourceTag/user-identity": "${aws:PrincipalTag/user-identity}"
                }
            },
            "Action" : "secretsmanager:GetSecretValue",
            "Resource" : [  "arn:aws:secretsmanager:us-west-2:AWS_ACCOUNT_ID:secret:aes123-1a2b3c", 
                            "arn:aws:secretsmanager:us-west-2:AWS_ACCOUNT_ID:secret:aes456-4d5e6f", 
                            "arn:aws:secretsmanager:us-west-2:AWS_ACCOUNT_ID:secret:aes789-7g8h9i" ]
        }
    ]
}
```