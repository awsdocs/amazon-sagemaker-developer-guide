# Security and Permissions<a name="data-wrangler-security"></a>

When you query data from Athena or Amazon Redshift, the queried dataset is automatically stored in the default SageMaker S3 bucket for the AWS Region in which you are using Studio\. Additionally, when you export a Jupyter Notebook from Amazon SageMaker Data Wrangler and execute it, your data flows, or \.flow files, are saved to the same default bucket, under the prefix *data\_wrangler\_flows*\.

For high\-level security needs, you can configure a bucket policy that restricts the AWS roles that have access to this default SageMaker S3 bucket\. Use the following section to add this type of policy to an S3 bucket\. To follow the instructions on this page, use the AWS Command Line Interface \(AWS CLI\)\. To learn how, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) in the IAM User Guide\.

Additionally, you need to grant each IAM role that uses Data Wrangler permissions to access required resources\. If you do not require granular permissions for the IAM role you use to access Data Wrangler, you can add the IAM managed policy, [https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess), to an IAM role that you use to create your Studio user\. This policy grants you full permission to use Data Wrangler\. If you require more granular permissions, refer to the section, [Grant an IAM Role Permission to Use Data Wrangler](#data-wrangler-security-iam-policy)\.

## Add a Bucket Policy To Restrict Access to Datasets Imported to Data Wrangler<a name="data-wrangler-security-bucket-policy"></a>

You can add a policy to the S3 bucket that contains your Data Wrangler resources using an Amazon S3 bucket policy\. Resources that Data Wrangler uploads to your default SageMaker S3 bucket in the AWS Region you are using Studio in include the following:
+ Queried Amazon Redshift results\. These are stored under the *redshift/* prefix\.
+ Queried Athena results\. These are stored under the *athena/* prefix\. 
+ The \.flow files uploaded to Amazon S3 when you execute an exported Jupyter Notebook Data Wrangler produces\. These are stored under the *data\_wrangler\_flows/* prefix\.

Use the following procedure to create an S3 bucket policy that you can add to restrict IAM role access to that bucket\. To learn how to add a policy to an S3 bucket, see [How do I add an S3 Bucket policy?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-bucket-policy.html)\.

**To set up a bucket policy on the S3 bucket that stores your Data Wrangler resources:**

1. Configure one or more IAM roles that you want to be able to access Data Wrangler\.

1. Open a command prompt or shell\. For each role that you create, replace *role\-name* with the name of the role and run the following:

   ```
   $ aws iam get-role --role-name role-name
   ```

   In the response, you'll see a `RoleId` string which begins will `AROA`\. Copy this string\. 

1. Add the following policy to the SageMaker default bucket in the AWS Region in which you are using Data Wrangler\. Replace *region* with the AWS Region in which the bucket is located, and *account\-id* with your AWS account ID\. Replace `userId`s starting with *AROAEXAMPLEID* with the IDs of an AWS roles to which you want to grant permission to use Data Wrangler\. 

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Deny",
         "Principal": "*",
         "Action": "s3:*",
         "Resource": [
           "arn:aws:s3:::sagemaker-region-account-id/data_wrangler_flows/",
           "arn:aws:s3:::sagemaker-region-account-id/data_wrangler_flows/*",
           "arn:aws:s3:::sagemaker-region-account-id/athena",
           "arn:aws:s3:::sagemaker-region-account-id/athena/*",
           "arn:aws:s3:::sagemaker-region-account-id/redshift",
           "arn:aws:s3:::sagemaker-region-account-id/redshift/*"
           
         ],
         "Condition": {
           "StringNotLike": {
             "aws:userId": [
               "AROAEXAMPLEID_1:*",
               "AROAEXAMPLEID_2:*"
             ]
           }
         }
       }
     ]
   }
   ```

## Grant an IAM Role Permission to Use Data Wrangler<a name="data-wrangler-security-iam-policy"></a>

You can grant an IAM role permission to use Data Wrangler with the general IAM managed policy, [https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess)\. This is a general policy that includes [permissions](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-amazonsagemakerfullaccess-policy) required to use all SageMaker services\. This policy grants an IAM role full access to Data Wrangler\. You should be aware of the following when using `AmazonSageMakerFullAccess` to grant access to Data Wrangler:
+ If you import data from Amazon Redshift, the **Database User** name must have the prefix `sagemaker_access`\.
+ This managed policy only grants permission to access buckets with one of the following in the name: `SageMaker`, `Sagemaker`, `sagemaker`, or `aws-glue`\. If want to use Data Wrangler to import from an S3 bucket without these phrases in the name, refer to the last section on this page to learn how to grant permission to an IAM entity to access your S3 buckets\.

If you have high\-security needs, you can attach the policies in this section to an IAM entity to grant permissions required to use Data Wrangler\.

If you have datasets in Amazon Redshift or Athena that an IAM role needs to import from Data Wrangler, you must add a policy to that entity to access these resources\. The following policies are the most restrictive policies you can use to give an IAM role permission to import data from Amazon Redshift and Athena\. 

To learn how to attach a custom policy to an IAM role, refer to [Managing IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html#create-managed-policy-console) in the IAM User Guide\.

**Policy example to grant access to an Athena dataset import**

The following policy assumes that the IAM role has permission to access the underlying S3 bucket where data is stored through a separate IAM policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "athena:ListDataCatalogs",
                "athena:ListDatabases",
                "athena:ListTableMetadata",
                "athena:GetQueryExecution",
                "athena:GetQueryResults",
                "athena:StartQueryExecution",
                "athena:StopQueryExecution"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateTable"
            ],
            "Resource": [
                "arn:aws:glue:*:*:table/*/sagemaker_tmp_*",
                "arn:aws:glue:*:*:table/sagemaker_featurestore/*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:DeleteTable"
            ],
            "Resource": [
                "arn:aws:glue:*:*:table/*/sagemaker_tmp_*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:GetDatabases",
                "glue:GetTable",
                "glue:GetTables"
            ],
            "Resource": [
                "arn:aws:glue:*:*:table/*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateDatabase",
                "glue:GetDatabase"
            ],
            "Resource": [
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/sagemaker_featurestore",
                "arn:aws:glue:*:*:database/sagemaker_processing",
                "arn:aws:glue:*:*:database/default",
                "arn:aws:glue:*:*:database/sagemaker_data_wrangler"
            ]
        }
    ]
}
```

****Policy example to grant access to an Amazon Redshift dataset import****

The following policy grants permission to set up an Amazon Redshift connection to Data Wrangler using database users that have the prefix `sagemaker_access` in the name\. To grant permission to connect using additional database users, add additional entries under `"Resources"` in the following policy\. The following policy assumes that the IAM role has permission to access the underlying S3 bucket where data is stored through a separate IAM policy, if applicable\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "redshift-data:ExecuteStatement",
                "redshift-data:DescribeStatement",
                "redshift-data:CancelStatement",
                "redshift-data:GetStatementResult",
                "redshift-data:ListSchemas",
                "redshift-data:ListTables"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "redshift:GetClusterCredentials"
            ],
            "Resource": [
                "arn:aws:redshift:*:*:dbuser:*/sagemaker_access*",
                "arn:aws:redshift:*:*:dbname:*"
            ]
        }
    ]
}
```

**Policy to grant access to an S3 bucket**

If your dataset is stored in Amazon S3, you can grant an IAM role permission to access this bucket with a policy similar to the following\. This example grants programmatic read\-write access to the bucket named *test*:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": ["arn:aws:s3:::test"]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": ["arn:aws:s3:::test/*"]
    }
  ]
}
```

To import data from Athena and Amazon Redshift, you must grant an IAM role permission to access the following prefixes under the default Amazon S3 bucket in the AWS Region Data Wrangler is being used in: `athena/`, `redshift/`\. If a default Amazon S3 bucket does not already exist in the AWS Region, you must also give the IAM role permission to create a bucket in this region\.

Additionally, if you want the IAM role to be able to use the SageMaker Feature Store, SageMaker Pipeline, and Data Wrangler job export options, you must grant access to the prefix `data_wrangler_flows/` in this bucket\.

 Data Wrangler uses the `athena/` and `redshift/` prefixes to store preview files and imported datasets\. To learn more, see [Imported Data Storage](data-wrangler-import.md#data-wrangler-import-storage)\.

Data Wrangler uses the `data_wrangler_flows/` prefix to store \.flow files when you run a Jupyter Notebook exported from Data Wrangler\. To learn more, see [Export](data-wrangler-data-export.md)\.

Use a policy similar to the following to grant the permissions described above:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::sagemaker-region-account-id/data_wrangler_flows/",
                "arn:aws:s3:::sagemaker-region-account-id/data_wrangler_flows/*",
                "arn:aws:s3:::sagemaker-region-account-id/athena",
                "arn:aws:s3:::sagemaker-region-account-id/athena/*",
                "arn:aws:s3:::sagemaker-region-account-id/redshift",
                "arn:aws:s3:::sagemaker-region-account-id/redshift/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::sagemaker-region-account-id"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets",
                "s3:GetBucketLocation"
            ],
            "Resource": "*"
        }
    ]
}
```

You can also access data in your Amazon S3 bucket from another AWS account by specifying the Amazon S3 bucket URI\. To do this, the IAM policy that grants access to the Amazon S3 bucket in the other account should use a policy similar to this below where `BucketFolder` is the specific folder in the users bucket `UserBucket`\. This policy should be added to the user granting access to their bucket for another user\. 

```
{
   "Version": "2012-10-17",
   "Statement": [
       {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::UserBucket/BucketFolder/*"
            },
                {
                "Effect": "Allow",
                "Action": [
                    "s3:ListBucket"
                ],
                "Resource": "arn:aws:s3:::UserBucket",
                "Condition": {
                "StringLike": {
                    "s3:prefix": [
                    "BucketFolder/*"
                    ]
                }
            }
        } 
    ]
}
```

The user that will be accessing bucket \(not the bucket owner\) will need to add a policy similar to this below to their user\. Note that `AccountX` and `TestUser` below refers to the bucket owner and their User respectively\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::AccountX:user/TestUser"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::UserBucket/BucketFolder/*"
            ]
        },
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::AccountX:user/TestUser"
            },
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::UserBucket"
            ]
        }
    ]
}
```

**Policy example to grant access to use SageMaker Studio**

Use a policy like to the following to create an IAM execution role that can be used to set up a Studio instance\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedDomainUrl",
                "sagemaker:DescribeDomain",
                "sagemaker:ListDomains",
                "sagemaker:DescribeUserProfile",
                "sagemaker:ListUserProfiles",
                "sagemaker:*App",
                "sagemaker:ListApps"
            ],
            "Resource": "*"
        }
    ]
}
```