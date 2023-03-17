# Configure a SageMaker Clarify Processing Job<a name="clarify-processing-job-configure-parameters"></a>

To analyze your data and models for bias and explainability using SageMaker Clarify, you must configure a Clarify processing job\. This guide shows how to specify the input dataset name, analysis configuration file name, and output location for a processing job\. To configure the processing container, job inputs, outputs, resources and other parameters, you have two options\. You can either use the SageMaker `CreateProcessingJob` API, or use the SageMaker Python SDK API `SageMaker ClarifyProcessor`,

For information about parameters that are common to all processing jobs, see [Amazon SageMaker API Reference](https://docs.aws.amazon.com/sagemaker/latest/APIReference/Welcome.html?icmpid=docs_sagemaker_lp)\.

## Configure a Clarify processing job using the SageMaker API<a name="clarify-processing-job-configure-parameters-API"></a>

The following instructions show how to provide each portion of the Clarify specific configuration using the `CreateProcessingJob` API\.

1. Input the uniform research identifier \(URI\) of a Clarify container image inside the `AppSpecification` parameter, as shown in the following code example\.

   ```
   {
       "ImageUri": "the-clarify-container-image-uri"
   }
   ```
**Note**  
The URI must identify a pre\-built [SageMaker Clarify container image](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-processing-job-configure-container.html)\. `ContainerEntrypoint` and `ContainerArguments` are not supported\.

1. Specify both the configuration for your analysis and parameters for your input dataset inside the `ProcessingInputs` parameter\.

   1. Specify the location of the JSON analysis configuration file, which includes the parameters for bias analysis and explainability analysis\. The `InputName` parameter of the `ProcessingInput` object must be **analysis\_config** as shown in the following code example\.

      ```
      {
          "InputName": "analysis_config",
          "S3Input": {
              "S3Uri": "s3://your-bucket/analysis_config.json",
              "S3DataType": "S3Prefix",
              "S3InputMode": "File",
              "LocalPath": "/opt/ml/processing/input/config"
          }
      }
      ```

      For more information about the schema of the analysis configuration file, see [Configure the Analysis](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-processing-job-configure-analysis.html)\.

   1. Specify the location of the input dataset\. The `InputName` parameter of the `ProcessingInput` object must be "dataset"\. This parameter is optional if you have provided the "dataset\_uri" in the analysis configuration file\. The following values are required in the `S3Input` configuration\.

      1. `S3Uri`can be either an Amazon S3 object or an S3 prefix\.

      1. `S3InputMode` must be of type **File**\.

      1. `S3CompressionType` must be of type `None` \(the default value\)\.

      1. `S3DataDistributionType` must be of type `FullyReplicated` \(the default value\)\.

      1. `S3DataType` can be either `S3Prefix` or `ManifestFile`\. To use `ManifestFile`, the `S3Uri` parameter should specify the location of a manifest file that follows the schema from the SageMaker API Reference section [S3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3DataSource.html#sagemaker-Type-S3DataSource-S3Uri)\. This manifest file must list the S3 objects that contain the input data for the job\.

      The following code shows an example of an input configuration\.

      ```
      {
          "InputName": "dataset",
          "S3Input": {
              "S3Uri": "s3://your-bucket/your-dataset.csv",
              "S3DataType": "S3Prefix",
              "S3InputMode": "File",
              "LocalPath": "/opt/ml/processing/input/data"
          }
      }
      ```

1. Specify the configuration for the output of the processing job inside the `ProcessingOutputConfig` parameter\. A single `ProcessingOutput` object is required in the `Outputs` configuration\. The following are required of the output configuration:

   1. `OutputName` must be **analysis\_result**

   1. `S3Uri`must be an S3 prefix to the output location

   1. `S3UploadMode` must be set to **EndOfJob**

   The following code shows an example of an output configuration\.

   ```
   {
       "Outputs": [{ 
           "OutputName": "analysis_result",
           "S3Output": { 
               "S3Uri": "s3://your-bucket/result/",
               "S3UploadMode": "EndOfJob",
               "LocalPath": "/opt/ml/processing/output"
            }
        }]
   }
   ```

1. Specify the configuration `ClusterConfig` for the resources that you use in your processing job inside the `ProcessingResources` parameter\. The following parameters are required inside the `ClusterConfig` object\.

   1. `InstanceCount` specifies the number of compute instances in the cluster that runs the processing job\. Specify a value greater than `1` to activate distributed processing\.

   1. `InstanceType` refers to the resources that runs your processing job\. Because SageMaker SHAP analysis is compute\-intensive, using an instance type that is optimized for compute should improve runtime for analysis\. The Clarify processing job doesn't use GPUs\.

   The following code shows an example of resource configuration\.

   ```
   {
       "ClusterConfig": {
            "InstanceCount": 1,
            "InstanceType": "ml.m5.xlarge",
            "VolumeSizeInGB": 20
        }
   }
   ```

1. Specify the configuration of the network that you use in your processing job inside the `NetworkConfig` object\. The following values are required in the configuration\.

   1. `EnableNetworkIsolation` must be set to `False` \(default\) so that Clarify can invoke an endpoint, if necessary, for predictions\.

   1. If the model or endpoint that you provided to the Clarify job is within an Amazon Virtual Private Cloud \(Amazon VPC\), then the Clarify job must also be in the same VPC\. Specify the VPC using [VpcConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_VpcConfig.html)\. Additionally, the VPC must have endpoints to an Amazon S3 bucket, SageMaker service and SageMaker Runtime service\.

      If distributed processing is activated, you must also allow communication between different instances in the same processing job\. Configure a rule for your security group that allows inbound connections between members of the same security group\. For more information, see [Give Amazon SageMaker Clarify Jobs Access to Resources in Your Amazon VPC](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-vpc.html)\. 

   The following code gives an example of a network configuration\.

   ```
   {
       "EnableNetworkIsolation": False,
       "VpcConfig": {
           ...
       }
   }
   ```

1. Set the maximum time that the job will run using the `StoppingCondition` parameter\. The longest that a Clarify job can run is `7` days or `604800` seconds\. If the job cannot be completed within this time limit, it will be stopped and no analysis results will be provided\. As an example, the following configuration limits the maximum time that the job can run to 3600 seconds\.

   ```
   {
       "MaxRuntimeInSeconds": 3600
   }
   ```

1. Specify an IAM role for the `RoleArn` parameter\. The role must have a trust relationship with Amazon SageMaker\. It can be used to perform the SageMaker API operations listed in the following table\. We recommend using the [Amazon SageMakerFullAccess](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol.html#security-iam-awsmanpol-AmazonSageMakerFullAccess) managed policy, which grants full access to SageMaker\. If you have concerns about granting full access, the minimal permissions required depend on whether you provide a model or an endpoint name\. Using an endpoint name allows for granting fewer permissions to SageMaker\.

   The following table contains API operations used by the Clarify processing job\. An **X** under **Model name** and **Endpoint name** notes the API operation that is required for each input\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/clarify-processing-job-configure-parameters.html)

   For more information about required permissions, see [Amazon SageMaker API Permissions: Actions, Permissions, and Resources Reference](https://docs.aws.amazon.com/sagemaker/latest/dg/api-permissions-reference.html)\.

   For more information about passing roles to SageMaker, see [Passing Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-pass-role)\.

   After you have the individual pieces of the processing job configuration, combine them to configure the job\.

## Configure a Clarify processing job using the AWS SDK for Python<a name="clarify-processing-job-configure-parameters-SDK"></a>

The following code example shows how to launch a Clarify processing job using the [AWS SDK for Python](http://aws.amazon.com/sdk-for-python/)\.

```
sagemaker_client.create_processing_job(
    ProcessingJobName="your-clarify-job-name",
    AppSpecification={
        "ImageUri": "the-clarify-container-image-uri",
    },
    ProcessingInputs=[{
            "InputName": "analysis_config",
            "S3Input": {
                "S3Uri": "s3://your-bucket/analysis_config.json",
                "S3DataType": "S3Prefix",
                "S3InputMode": "File",
                "LocalPath": "/opt/ml/processing/input/config",
            },
        }, {
            "InputName": "dataset",
            "S3Input": {
                "S3Uri": "s3://your-bucket/your-dataset.csv",
                "S3DataType": "S3Prefix",
                "S3InputMode": "File",
                "LocalPath": "/opt/ml/processing/input/data",
            },
        },
    ],
    ProcessingOutputConfig={
        "Outputs": [{ 
            "OutputName": "analysis_result",
            "S3Output": { 
               "S3Uri": "s3://your-bucket/result/",
               "S3UploadMode": "EndOfJob",
               "LocalPath": "/opt/ml/processing/output",
            },   
        }],
    },
    ProcessingResources={
        "ClusterConfig": {
            "InstanceCount": 1,
            "InstanceType": "ml.m5.xlarge",
            "VolumeSizeInGB": 20,
        },
    },
    NetworkConfig={
        "EnableNetworkIsolation": False,
        "VpcConfig": {
            ...
        },
    },
    StoppingCondition={
        "MaxRuntimeInSeconds": 3600,
    },
    RoleArn="arn:aws:iam::<your-account-id>:role/service-role/AmazonSageMaker-ExecutionRole",
)
```

For an example notebook with instructions for running a SageMaker Clarify processing job using AWS SDK for Python, see [Fairness and Explainability with SageMaker Clarify using AWS SDK for Python](http://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-clarify/fairness_and_explainability/fairness_and_explainability_boto3.ipynb)\. Any S3 bucket used in the notebook must be in the same AWS Region as the notebook instance that accesses the it\.

## Configure a Clarify processing job using the SageMaker Python SDK<a name="clarify-processing-job-configure-parameters-SM-SDK"></a>

You can also configure a Clarify processing job using the [SageMaker ClarifyProcessor](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.clarify.SageMakerClarifyProcessor) in the SageMaker Python SDK API\. For more information, see [Run SageMaker Clarify Processing Jobs for Bias Analysis and Explainability](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-processing-job-run.html)\.