# Configure AWS Service Catalog Products<a name="configure-service-catalog-templates"></a>

In this guide, you get parameters for configuring AWS Service Catalog products so you can discover CloudFormation \(CFN\) templates\. CFN templates are used to create and manage Amazon EMR clusters\. The AWS Service Catalog product can associate with an AWS Service Catalog portfolio that is shared with the Studio execution role IAM entity\.

To enable Amazon EMR cluster management from Studio, you need a CloudFormation \(CFN\) template with Amazon EMR cluster details and configurable parameters\. For information about how you can use CloudFormation, see [Getting started with CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html)\. For more information about AWS Service Catalog products, see [Step 4: Create an AWS Service Catalog Product](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/getstarted-product.html)\.

The AWS Service Catalog product with the Amazon EMR resource must have the following tags: 

```
sagemaker:studio-visibility:emr true
```

The CFN templates in the AWS Service Catalog product must have the following mandatory stack parameters:

```
SageMakerProjectName:
Type: String
Description: Name of the project

SageMakerProjectId:
Type: String
Description: Service generated Id of the project.
```

For more information about creating AWS Service Catalog portfolios and products, see [Step 3: Create an AWS Service Catalog Portfolio](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/getstarted-portfolio.html) and its subsequent sections\.

**Optional template parameters**

When creating Amazon EMR templates from within SageMaker Studio, you can include additional options in the parameters section of your template\. This section lets users input or select custom values for a cluster\. The following example parameters define additional input parameters that you can use when creating an Amazon EMR template from within Studio\. 

```
"Parameters": {
    "EmrClusterName": {
      "Type": "String",
      "Description": "EMR cluster Name."
    },
    "MasterInstanceType": {
      "Type": "String",
      "Description": "Instance type of the EMR master node.",
      "Default": "m5.xlarge",
      "AllowedValues": [
        "m5.xlarge",
        "m5.2xlarge",
        "m5.4xlarge"
      ]
    },
    "CoreInstanceType": {
      "Type": "String",
      "Description": "Instance type of the EMR core nodes.",
      "Default": "m5.xlarge",
      "AllowedValues": [
        "m5.xlarge",
        "m5.2xlarge",
        "m5.4xlarge",
        "m3.medium",
        "m3.large",
        "m3.xlarge",
        "m3.2xlarge"
      ]
    },
    "CoreInstanceCount": {
      "Type": "String",
      "Description": "Number of core instances in the EMR cluster.",
      "Default": "2",
      "AllowedValues": [
        "2",
        "5",
        "10"
      ]
    },
    "EmrReleaseVersion": {
      "Type": "String",
      "Description": "The release version of EMR to launch.",
      "Default": "emr-5.33.1",
      "AllowedValues": [
        "emr-5.33.1",
        "emr-6.4.0"
      ]
    }
  }
```

The following image shows how the Studio UI will look when you create an Amazon EMR cluster from Studio\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/emr-create-cluster-details-service-catalog.png)