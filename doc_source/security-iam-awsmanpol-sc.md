# AWS Managed Policies for SageMaker projects and JumpStart<a name="security-iam-awsmanpol-sc"></a>

These AWS managed policies add permissions to use built\-in Amazon SageMaker project templates and JumpStart solutions\. The policies are available in your AWS account and are used by execution roles created from the SageMaker console\.

SageMaker projects and JumpStart use AWS Service Catalog to provision AWS resources in customers' accounts\. Some created resources need to assume an execution role\. For example, if AWS Service Catalog creates a CodePipeline pipeline on behalf of a customer for a SageMaker machine learning CI/CD project, then that pipeline requires an IAM role\.

The [AmazonSageMakerServiceCatalogProductsLaunchRole](https://console.aws.amazon.com/iam/home?#/roles/AmazonSageMakerServiceCatalogProductsLaunchRole) role has the permissions required to launch the SageMaker portfolio of products from AWS Service Catalog\. The [AmazonSageMakerServiceCatalogProductsUseRole](https://console.aws.amazon.com/iam/home?#/roles/AmazonSageMakerServiceCatalogProductsUseRole) role has the permissions required to use the SageMaker portfolio of products from AWS Service Catalog\. The `AmazonSageMakerServiceCatalogProductsLaunchRole` role passes an `AmazonSageMakerServiceCatalogProductsUseRole` role to the provisioned AWS Service Catalog product resources\.

**Topics**
+ [AWS managed policy: AmazonSageMakerAdmin\-ServiceCatalogProductsServiceRolePolicy](#security-iam-awsmanpol-AmazonSageMakerAdmin-ServiceCatalogProductsServiceRolePolicy)
+ [Amazon SageMaker updates to AWS Service Catalog AWS managed policies](#security-iam-awsmanpol-sc-updates)

## AWS managed policy: AmazonSageMakerAdmin\-ServiceCatalogProductsServiceRolePolicy<a name="security-iam-awsmanpol-AmazonSageMakerAdmin-ServiceCatalogProductsServiceRolePolicy"></a>

This service role policy is used by the AWS Service Catalog service to provision products from the Amazon SageMaker portfolio\. The policy grants permissions to a set of related AWS services including AWS CodePipeline, AWS CodeBuild, AWS CodeCommit, AWS Glue, Amazon CloudFront, and others\.

The `AmazonSageMakerAdmin-ServiceCatalogProductsServiceRolePolicy` policy is intended to be used by the `AmazonSageMakerServiceCatalogProductsLaunchRole` role created from the SageMaker console\. The policy adds permissions to provision AWS resources for SageMaker projects and JumpStart using AWS Service Catalog to a customer's account\.

**Permissions details**

This policy includes the following permissions\.
+ `apigateway` – Allows the role to call API Gateway endpoints that are tagged with `sagemaker:launch-source`\.
+ `cloudformation` – Allows AWS Service Catalog to create, update and delete CloudFormation stacks\.
+ `codebuild` – Allows the role assumed by AWS Service Catalog and passed to CloudFormation to create, update and delete CodeBuild projects\.
+ `codecommit` – Allows the role assumed by AWS Service Catalog and passed to CloudFormation to create, update and delete CodeCommit repositories\.
+ `codepipeline` – Allows the role assumed by AWS Service Catalog and passed to CloudFormation to create, update and delete CodePipelines\.
+ `codestar-connections` – Allows the role to pass AWS CodeStar connections\.
+ `cognito-idp` – Allows the role to create, update, and delete groups and user pools\. Also allows tagging resources\.
+ `ecr` – Allows the role assumed by AWS Service Catalog and passed to CloudFormation to create and delete Amazon ECR repositories\. Also allows tagging resources\.
+ `events` – Allows the role assumed by AWS Service Catalog and passed to CloudFormation to create and delete EventBridge rules\. Used for tying together the various components of the CICD pipeline\.
+ `firehose` – Allows the role to interact with Kinesis Data Firehose streams\.
+ `glue` – Allows the role to interact with AWS Glue\.
+ `iam` – Allows the role to pass roles prepended with `AmazonSageMakerServiceCatalog`\. This is needed when Projects provisions a AWS Service Catalog product, as a role needs to be passed to AWS Service Catalog\.
+ `lambda` – Allows the role to interact with AWS Lambda\. Also allows tagging resources\.
+ `logs` – Allows the role to create, delete and access log streams\.
+ `s3` – Allows the role assumed by AWS Service Catalog and passed to CloudFormation to access Amazon S3 buckets where the Project template code is stored\.
+ `sagemaker` – Allows the role to interact with various SageMaker services\. This is done both in CloudFormation during template provisioning, as well as in CodeBuild during CICD pipeline execution\.
+ `states` – Allows the role to create, delete, and update Step Functions prepended with `sagemaker`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "apigateway:GET",
        "apigateway:POST",
        "apigateway:PUT",
        "apigateway:PATCH",
        "apigateway:DELETE"
      ],
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "aws:ResourceTag/sagemaker:launch-source": "*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "apigateway:POST"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringLike": {
          "aws:TagKeys": [
            "sagemaker:launch-source"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "apigateway:PATCH"
      ],
      "Resource": [
        "arn:aws:apigateway:*::/account"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CreateStack",
        "cloudformation:UpdateStack",
        "cloudformation:DeleteStack"
      ],
      "Resource": "arn:aws:cloudformation:*:*:stack/SC-*",
      "Condition": {
        "ArnLikeIfExists": {
          "cloudformation:RoleArn": [
            "arn:aws:sts::*:assumed-role/AmazonSageMakerServiceCatalog*"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:DescribeStackEvents",
        "cloudformation:DescribeStacks"
      ],
      "Resource": "arn:aws:cloudformation:*:*:stack/SC-*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:GetTemplateSummary",
        "cloudformation:ValidateTemplate"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "codebuild:CreateProject",
        "codebuild:DeleteProject",
        "codebuild:UpdateProject"
      ],
      "Resource": [
        "arn:aws:codebuild:*:*:project/sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "codecommit:CreateCommit",
        "codecommit:CreateRepository",
        "codecommit:DeleteRepository",
        "codecommit:GetRepository",
        "codecommit:TagResource"
      ],
      "Resource": [
        "arn:aws:codecommit:*:*:sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "codecommit:ListRepositories"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "codepipeline:CreatePipeline",
        "codepipeline:DeletePipeline",
        "codepipeline:GetPipeline",
        "codepipeline:GetPipelineState",
        "codepipeline:StartPipelineExecution",
        "codepipeline:TagResource",
        "codepipeline:UpdatePipeline"
      ],
      "Resource": [
        "arn:aws:codepipeline:*:*:sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "cognito-idp:CreateUserPool",
        "cognito-idp:TagResource"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringLike": {
          "aws:TagKeys": [
            "sagemaker:launch-source"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "cognito-idp:CreateGroup",
        "cognito-idp:CreateUserPoolDomain",
        "cognito-idp:CreateUserPoolClient",
        "cognito-idp:DeleteGroup",
        "cognito-idp:DeleteUserPool",
        "cognito-idp:DeleteUserPoolClient",
        "cognito-idp:DeleteUserPoolDomain",
        "cognito-idp:DescribeUserPool",
        "cognito-idp:DescribeUserPoolClient",
        "cognito-idp:UpdateUserPool",
        "cognito-idp:UpdateUserPoolClient"
      ],
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "aws:ResourceTag/sagemaker:launch-source": "*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "ecr:CreateRepository",
        "ecr:DeleteRepository",
        "ecr:TagResource"
      ],
      "Resource": [
        "arn:aws:ecr:*:*:repository/sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "events:DescribeRule",
        "events:DeleteRule",
        "events:DisableRule",
        "events:EnableRule",
        "events:PutRule",
        "events:PutTargets",
        "events:RemoveTargets"
      ],
      "Resource": [
        "arn:aws:events:*:*:rule/sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "firehose:CreateDeliveryStream",
        "firehose:DeleteDeliveryStream",
        "firehose:DescribeDeliveryStream",
        "firehose:StartDeliveryStreamEncryption",
        "firehose:StopDeliveryStreamEncryption",
        "firehose:UpdateDestination"
      ],
      "Resource": "arn:aws:firehose:*:*:deliverystream/sagemaker-*"
    },
    {
        "Effect": "Allow",
      "Action": [
        "glue:CreateDatabase",
        "glue:DeleteDatabase"
      ],
      "Resource": [
        "arn:aws:glue:*:*:catalog",
        "arn:aws:glue:*:*:database/sagemaker-*",
        "arn:aws:glue:*:*:table/sagemaker-*",
        "arn:aws:glue:*:*:userDefinedFunction/sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateClassifier",
        "glue:DeleteClassifier",
        "glue:DeleteCrawler",
        "glue:DeleteJob",
        "glue:DeleteTrigger",
        "glue:DeleteWorkflow",
        "glue:StopCrawler"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateWorkflow"
      ],
      "Resource": [
        "arn:aws:glue:*:*:workflow/sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateJob"
      ],
      "Resource": [
        "arn:aws:glue:*:*:job/sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateCrawler",
        "glue:GetCrawler"
      ],
      "Resource": [
        "arn:aws:glue:*:*:crawler/sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateTrigger",
        "glue:GetTrigger"
      ],
      "Resource": [
        "arn:aws:glue:*:*:trigger/sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": [
        "arn:aws:iam::*:role/service-role/AmazonSageMakerServiceCatalog*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "lambda:AddPermission",
        "lambda:CreateFunction",
        "lambda:DeleteFunction",
        "lambda:GetFunction",
        "lambda:GetFunctionConfiguration",
        "lambda:InvokeFunction",
        "lambda:RemovePermission"
      ],
      "Resource": [
        "arn:aws:lambda:*:*:function:sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "lambda:TagResource",
      "Resource": [
        "arn:aws:lambda:*:*:function:sagemaker-*"
      ],
      "Condition": {
        "ForAllValues:StringLike": {
          "aws:TagKeys": [
            "sagemaker:*"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:DeleteLogGroup",
        "logs:DeleteLogStream",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams",
        "logs:PutRetentionPolicy"
      ],
      "Resource": [
        "arn:aws:logs:*:*:log-group:/aws/apigateway/AccessLogs/*",
        "arn:aws:logs:*:*:log-group::log-stream:*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "s3:ExistingObjectTag/servicecatalog:provisioning": "true"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": [
        "arn:aws:s3:::sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:DeleteBucketPolicy",
        "s3:GetBucketPolicy",
        "s3:PutBucketAcl",
        "s3:PutBucketNotification",
        "s3:PutBucketPolicy",
        "s3:PutBucketPublicAccessBlock",
        "s3:PutBucketLogging",
        "s3:PutEncryptionConfiguration",
        "s3:PutBucketTagging",
        "s3:PutObjectTagging",
        "s3:PutBucketCORS"
      ],
      "Resource": "arn:aws:s3:::sagemaker-*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "sagemaker:CreateEndpoint",
        "sagemaker:CreateEndpointConfig",
        "sagemaker:CreateModel",
        "sagemaker:CreateWorkteam",
        "sagemaker:DeleteEndpoint",
        "sagemaker:DeleteEndpointConfig",
        "sagemaker:DeleteModel",
        "sagemaker:DeleteWorkteam",
        "sagemaker:DescribeModel",
        "sagemaker:DescribeEndpointConfig",
        "sagemaker:DescribeEndpoint",
        "sagemaker:DescribeWorkteam",
        "sagemaker:CreateCodeRepository",
        "sagemaker:DescribeCodeRepository",
        "sagemaker:UpdateCodeRepository",
        "sagemaker:DeleteCodeRepository"
      ],
      "Resource": [
        "arn:aws:sagemaker:*:*:*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "sagemaker:CreateImage",
        "sagemaker:DeleteImage",
        "sagemaker:DescribeImage",
        "sagemaker:UpdateImage",
        "sagemaker:ListTags"
      ],
      "Resource": [
        "arn:aws:sagemaker:*:*:image/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "states:CreateStateMachine",
        "states:DeleteStateMachine",
        "states:UpdateStateMachine"
      ],
      "Resource": [
        "arn:aws:states:*:*:stateMachine:sagemaker-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "codestar-connections:PassConnection",
      "Resource": "arn:aws:codestar-connections:*:*:connection/*",
      "Condition": {
        "StringEquals": {
          "codestar-connections:PassedToService": "codepipeline.amazonaws.com"
        }
      }
    }
  ]
}
```

## Amazon SageMaker updates to AWS Service Catalog AWS managed policies<a name="security-iam-awsmanpol-sc-updates"></a>

View details about updates to AWS managed policies for Amazon SageMaker since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the SageMaker [Document history page\.](doc-history.md)


| Policy | Version | Change | Date | 
| --- | --- | --- | --- | 
|   [AmazonSageMakerAdmin\-ServiceCatalogProductsServiceRolePolicy](#security-iam-awsmanpol-AmazonSageMakerAdmin-ServiceCatalogProductsServiceRolePolicy)  | 6 |  Add permission for`lambda:TagResource`\.  | July 14, 2022 | 
| AmazonSageMakerAdmin\-ServiceCatalogProductsServiceRolePolicy | 5 |  Add new permission for `ecr-idp:TagResource`\.  | March 21, 2022 | 
| AmazonSageMakerAdmin\-ServiceCatalogProductsServiceRolePolicy | 4 |  Add new permissions for `cognito-idp:TagResource` and `s3:PutBucketCORS`\.  | February 16, 2022 | 
| AmazonSageMakerAdmin\-ServiceCatalogProductsServiceRolePolicy | 3 |  Add new permissions for `sagemaker`\. Create, read, update, and delete SageMaker Images\.  | September 15, 2021 | 
| AmazonSageMakerAdmin\-ServiceCatalogProductsServiceRolePolicy | 2 |  Add new permissions for `sagemaker` and `codestar-connections`\. Create, read, update, and delete code repositories\. Pass AWS CodeStar connections to AWS CodePipeline\.  | July 1, 2021 | 
| AmazonSageMakerAdmin\-ServiceCatalogProductsServiceRolePolicy | 1 |  Initial policy     | November 27, 2020 | 