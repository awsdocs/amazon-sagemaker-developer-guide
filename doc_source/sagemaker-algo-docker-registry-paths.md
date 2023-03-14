# Docker Registry Paths and Example Code<a name="sagemaker-algo-docker-registry-paths"></a>

The following topics list the Docker registry path and other parameters for each of the Amazon SageMaker provided algorithms and Deep Learning Containers \(DLC\)\.

Use the path as follows:
+ To create a training job \([create\_training\_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job)\), specify the Docker registry path \(`TrainingImage`\) and the training input mode \(`TrainingInputMode`\) for the training image\. You create a training job to train a model using a specific dataset\.
+ To create a model \([create\_model](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_model)\), specify the Docker registry path \(`Image`\) for the inference image \(`PrimaryContainer Image`\)\. SageMaker launches machine learning compute instances that are based on the endpoint configuration and deploys the model, which includes the artifacts \(the result of model training\)\.

**Note**  
For the registry path, use the `:1` version tag to ensure that you are using a stable version of the algorithm/DLC\. You can reliably host a model trained using an image with the `:1` tag on an inference image that has the `:1` tag\. Using the `:latest` tag in the registry path provides you with the most up\-to\-date version of the algorithm/DLC, but might cause problems with backward compatibility\. Avoid using the `:latest` tag for production purposes\.

**Important**  
When you retrieve the SageMaker XGBoost image URI, do not use `:latest` or `:1` for the image URI tag\. You must specify one of the [Supported versions](xgboost.md#xgboost-supported-versions) to choose the SageMaker\-managed XGBoost container with the native XGBoost package version that you want to use\. To find the package version migrated into the SageMaker XGBoost containers, see [Docker Registry Paths and Example Code](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html), choose your AWS Region, and navigate to the **XGBoost \(algorithm\)** section\.

To find the registry path, choose the AWS Region, then choose the algorithm or DLC\.

**Topics**
+ [US East \(Ohio\)](ecr-us-east-2.md)
+ [US East \(N\. Virginia\)](ecr-us-east-1.md)
+ [US West \(N\. California\)](ecr-us-west-1.md)
+ [US West \(Oregon\)](ecr-us-west-2.md)
+ [Africa \(Cape Town\)](ecr-af-south-1.md)
+ [Asia Pacific \(Hong Kong\)](ecr-ap-east-1.md)
+ [Asia Pacific \(Mumbai\)](ecr-ap-south-1.md)
+ [Asia Pacific \(Osaka\)](ecr-ap-northeast-3.md)
+ [Asia Pacific \(Seoul\)](ecr-ap-northeast-2.md)
+ [Asia Pacific \(Singapore\)](ecr-ap-southeast-1.md)
+ [Asia Pacific \(Sydney\)](ecr-ap-southeast-2.md)
+ [Asia Pacific \(Jakarta\)](ecr-ap-southeast-3.md)
+ [Asia Pacific \(Tokyo\)](ecr-ap-northeast-1.md)
+ [Canada \(Central\)](ecr-ca-central-1.md)
+ [China \(Beijing\)](ecr-cn-north-1.md)
+ [China \(Ningxia\)](ecr-cn-northwest-1.md)
+ [Europe \(Frankfurt\)](ecr-eu-central-1.md)
+ [Europe \(Ireland\)](ecr-eu-west-1.md)
+ [Europe \(London\)](ecr-eu-west-2.md)
+ [Europe \(Paris\)](ecr-eu-west-3.md)
+ [Europe \(Stockholm\)](ecr-eu-north-1.md)
+ [Europe \(Milan\)](ecr-eu-south-1.md)
+ [Middle East \(Bahrain\)](ecr-me-south-1.md)
+ [South America \(São Paulo\)](ecr-sa-east-1.md)
+ [AWS GovCloud \(US\-West\)](ecr-us-gov-west-1.md)