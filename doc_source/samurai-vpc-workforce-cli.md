# Using the SageMaker AWS API to manage a VPC config<a name="samurai-vpc-workforce-cli"></a>

Download the following files to use a new `VpcConfig` parameter into to the SageMaker workforce CLI:

[sagemaker\-2017\-07\-24\.normal\.json](https://s3.amazonaws.com/sagemaker-sample-files/templates/groundtruth/sagemaker-2017-07-24.normal.json)

[sagemaker\-2017\-07\-24\.paginators\.json](https://s3.amazonaws.com/sagemaker-sample-files/templates/groundtruth/sagemaker-2017-07-24.paginators.json)

[sagemaker\-2017\-07\-24\.waiters\-2\.json](https://s3.amazonaws.com/sagemaker-sample-files/templates/groundtruth/sagemaker-2017-07-24.waiters-2.json)

After downloading the files, run the following commands in your CLI:

```
aws configure add-model --service-model file://./sagemaker-2017-07-24.normal.json --service-name sagemaker
```

```
cp ./sagemaker-2017-07-24.paginators.json ~/.aws/models/sagemaker/2017-07-24/paginators.json
```

```
cp ./sagemaker-2017-07-24.waiters-2.json ~/.aws/models/sagemaker/2017-07-24/waiters-2.json
```

You can now test your API changes using AWS CLI\. You can either create a new workforce with a VPC configuration or update an existing workforce to add a VPC configuration\. You can also remove a VPC configuration from an existing workforce\.

## Create a workforce with a VPC configuration<a name="samurai-create-vpc-cli"></a>

If the account already has a workforce, then you must delete it first\. You can also update the workforce with VPC configuration\.

```
aws sagemaker create-workforce --cognito-config '{"ClientId": "app-client-id","UserPool": "Pool_ID",}' --workforce-vpc-config \       
" {\"VpcId\": \"vpc-id\", \"SecurityGroupIds\": [\"sg-0123456789abcdef0\"], \"Subnets\": [\"subnet-0123456789abcdef0\"]}" --workforce-name workforce-name
{
    "WorkforceArn": "arn:aws:sagemaker:us-west-2:xxxxxxxxx:workforce/workforce-name"
}
```

Describe the workforce and make sure the status is `Initializing`\.

```
aws sagemaker describe-workforce --workforce-name workforce-name
{
    "Workforce": {
        "WorkforceName": "workforce-name",
        "WorkforceArn": "arn:aws:sagemaker:us-west-2:xxxxxxxxx:workforce/workforce-name",
        "LastUpdatedDate": 1622151252.451,
        "SourceIpConfig": {
            "Cidrs": []
        },
        "SubDomain": "subdomain.us-west-2.sagamaker.aws.com",
        "CognitoConfig": {
            "UserPool": "Pool_ID",
            "ClientId": "app-client-id"
        },
        "CreateDate": 1622151252.451,
        "WorkforceVpcConfig": {
            "VpcId": "vpc-id",
            "SecurityGroupIds": [
                "sg-0123456789abcdef0"
            ],
            "Subnets": [
                "subnet-0123456789abcdef0"
            ]
        },
        "Status": "Initializing"
    }
}
```

Navigate to the Amazon VPC console\. Select **Endpoints** from the left panel\. There should be two VPC endpoints created in your account\.

## Adding a VPC configuration your workforce<a name="samurai-add-vpc-cli"></a>

Update a non\-VPC private workforce with a VPC configuration using the following command\.

```
aws sagemaker update-workforce --workforce-name workforce-name\
--workforce-vpc-config "{\"VpcId\": \"vpc-id\", \"SecurityGroupIds\": [\"sg-0123456789abcdef0\"], \"Subnets\": [\"subnet-0123456789abcdef0\"]}"
```

Describe the workforce and make sure the status is `Updating`\.

```
aws sagemaker describe-workforce --workforce-name workforce-name
{
    "Workforce": {
        "WorkforceName": "workforce-name",
        "WorkforceArn": "arn:aws:sagemaker:us-west-2:xxxxxxxxx:workforce/workforce-name",
        "LastUpdatedDate": 1622151252.451,
        "SourceIpConfig": {
            "Cidrs": []
        },
        "SubDomain": "subdomain.us-west-2.sagamaker.aws.com",
        "CognitoConfig": {
            "UserPool": "Pool_ID",
            "ClientId": "app-client-id"
        },
        "CreateDate": 1622151252.451,
        "WorkforceVpcConfig": {
            "VpcId": "vpc-id",
            "SecurityGroupIds": [
                "sg-0123456789abcdef0"
            ],
            "Subnets": [
                "subnet-0123456789abcdef0"
            ]
        },
        "Status": "Updating"
    }
}
```

Navigate to your Amazon VPC console\. Select **Endpoints** from the left panel\. There should be two VPC endpoints created in your account\.

## Removing a VPC configuration from your workforce<a name="samurai-remove-vpc-cli"></a>

Update a VPC private workforce with an empty VPC configuration to remove VPC resources\.

```
aws sagemaker update-workforce --workforce-name workforce-name\ 
--workforce-vpc-config "{}"
```

Describe the workforce and make sure the status is `Updating`\.

```
aws sagemaker describe-workforce --workforce-name workforce-name
{
    "Workforce": {
        "WorkforceName": "workforce-name",
        "WorkforceArn": "arn:aws:sagemaker:us-west-2:xxxxxxxxx:workforce/workforce-name",
        "LastUpdatedDate": 1622151252.451,
        "SourceIpConfig": {
            "Cidrs": []
        },
        "SubDomain": "subdomain.us-west-2.sagamaker.aws.com",
        "CognitoConfig": {
            "UserPool": "Pool_ID",
            "ClientId": "app-client-id"
        },
        "CreateDate": 1622151252.451,
        "Status": "Updating"
    }
}
```

Naviagate to your Amazon VPC console\. Select **Endpoints** from the left panel\. The two VPC endpoints should be deleted\.

## Restrict public access to the worker portal while maintaining access through a VPC<a name="public-access-vpc"></a>

 The workers in a VPC or non\-VPC worker portal are be able to see the labeling job tasks assigned to them\. The assignment comes from assigning workers in a work team through OIDC groups\. It is the customer’s responsibility to restrict the access to their public worker portal by setting the `sourceIpConfig` in their workforce\. 

**Note**  
You can restrict access to the worker portal only through the SageMaker API\. This cannot be done through the console\.

Use the following command to restrict public access to the worker portal\.

```
aws sagemaker update-workforce --region us-west-2 \
--workforce-name workforce-demo --source-ip-config '{"Cidrs":["10.0.0.0/16"]}'
```

After the `sourceIpConfig` is set on the workforce, the workers can access the worker portal in VPC but not through public internet\.

**Note**  
You can not set the `sourceIP` restriction for worker portal in VPC\.