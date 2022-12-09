# Prerequisites<a name="rstudio-byoi-prerequisites"></a>

You must complete the following prerequisites before bringing your own image to use with RStudio on Amazon SageMaker\. 
+ If you have an existing Domain with RStudio that was created before April 7, 2022, you must delete your RStudioServerPro application and recreate it\. For information about how to delete an application, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.
+ Install the Docker application\. For information about setting up Docker, see [Orientation and setup](https://docs.docker.com/get-started/)\.
+ Create a local copy of an RStudio\-compatible Dockerfile that works with SageMaker\. For information about creating a sample RStudio dockerfile, see [Use a custom image to bring your own development environment to RStudio on Amazon SageMaker](http://aws.amazon.com/blogs/machine-learning/use-a-custom-image-to-bring-your-own-development-environment-to-rstudio-on-amazon-sagemaker/)\.
+ Use an AWS Identity and Access Management execution role that has the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\. If you have onboarded to Domain, you can get the role from the **Domain Summary** section of the SageMaker control panel\.

  Add the following permissions to access the Amazon Elastic Container Registry \(Amazon ECR\) service to your execution role\.

  ```
  { 
      "Version":"2012-10-17", 
      "Statement":[ 
          {
              "Sid": "VisualEditor0",
              "Effect":"Allow", 
              "Action":[ 
                  "ecr:CreateRepository", 
                  "ecr:BatchGetImage", 
                  "ecr:CompleteLayerUpload", 
                  "ecr:DescribeImages", 
                  "ecr:DescribeRepositories", 
                  "ecr:UploadLayerPart", 
                  "ecr:ListImages", 
                  "ecr:InitiateLayerUpload", 
                  "ecr:BatchCheckLayerAvailability", 
                  "ecr:PutImage" 
              ], 
              "Resource": "*" 
          }
      ]
  }
  ```
+ Install and configure AWS CLI with the following \(or higher\) version\. For information about installing the AWS CLI, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

  ```
  AWS CLI v1 >= 1.23.6
  AWS CLI v2 >= 2.6.2
  ```