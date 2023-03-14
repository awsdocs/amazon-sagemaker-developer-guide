# Get compiled recommendations with Neo<a name="inference-recommender-neo-compilation"></a>

In Inference Recommender, you can compile your model with Neo and get endpoint recommendations for your compiled model\. [SageMaker Neo](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html) is a service that can optimize your model for a target hardware platform \(that is, a specific instance type or environment\)\. Optimizing a model with Neo might improve the performance of your hosted model\.

For Neo\-supported frameworks and containers, Inference Recommender automatically suggests Neo\-optimized recommendations\. To be eligible for Neo compilation, your input must meet the following prerequisites:
+ You are using a SageMaker owned [DLC ](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/what-is-dlc.html) or XGBoost container\.
+ You are using a framework version supported by Neo\. For the framework versions supported by Neo, see [Cloud Instances](neo-supported-cloud.md#neo-supported-cloud-instances) in the SageMaker Neo documentation\.
+ Neo requires that you provide a correct input data shape for your model\. You can specify this data shape as the `[DataInputConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelInput.html#sagemaker-Type-ModelInput-DataInputConfig)` in the `[InferenceSpecification](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelPackage.html#sagemaker-CreateModelPackage-request-InferenceSpecification)` when you create a model package\. For information about the correct data shapes for each framework, see [Prepare Model for Compilation](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-compilation-preparing-model.html) in the SageMaker Neo documentation\.

  The following example shows how to specify the `DataInputConfig` field in the `InferenceSpecification`, where `data_input_configuration` is a variable that contains the data shape in dictionary format \(for example, `{'input':[1,1024,1024,3]}`\)\.

  ```
  "InferenceSpecification": {
          "Containers": [
              {
                  "Image": dlc_uri,
                  "Framework": framework.upper(),
                  "FrameworkVersion": framework_version,
                  "NearestModelName": model_name,
                  "ModelInput": {"DataInputConfig": data_input_configuration},
              }
          ],
          "SupportedContentTypes": input_mime_types,  # required, must be non-null
          "SupportedResponseMIMETypes": [],
          "SupportedRealtimeInferenceInstanceTypes": supported_realtime_inference_types,  # optional
      }
  ```

If these conditions are met in your request, then Inference Recommender runs scenarios for both compiled and uncompiled versions of your model, giving you multiple recommendation combinations to choose from\. You can compare the configurations for compiled and uncompiled versions of the same instance recommendation and determine which one best suits your use case\. The recommendations are ranked by cost per inference\.

To get the Neo compilation recommendations, you don’t have to do any additional configuration besides making sure that your input meets the above requirements\. Inference Recommender automatically runs Neo compilation on your model if your input meets the requirements, and you’ll receive a response that includes Neo recommendations\.

If you run into errors during your Neo compilation, see [Troubleshoot Neo Compilation Errors](neo-troubleshooting-compilation.md)\.

The following table is an example of a response you might get from an Inference Recommender job that includes recommendations for compiled models\. If the `InferenceSpecificationName` field is `None`, then the recommendation is an uncompiled model\. The last row, where the value for the `InferenceSpecificationName` field is `neo-00011122-2333-4445-5566-677788899900`, is for a model compiled with Neo\. The value in the field is the name of the Neo job used to compile and optimize your model\.


| EndpointName | InstanceType | InitialInstanceCount | EnvironmentParameters | CostPerHour | CostPerInference | MaxInvocations | ModelLatency | InferenceSpecificationName | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| sm\-epc\-example\-000111222 | ml\.c5\.9xlarge | 1 | \[\] | 1\.836 | 9\.15E\-07 | 33456 | 7 | None | 
| sm\-epc\-example\-111222333 | ml\.c5\.2xlarge | 1 | \[\] | 0\.408 | 2\.11E\-07 | 32211 | 21 | None | 
| sm\-epc\-example\-222333444 | ml\.c5\.xlarge | 1 | \[\] | 0\.204 | 1\.86E\-07 | 18276 | 92 | None | 
| sm\-epc\-example\-333444555 | ml\.c5\.xlarge | 1 | \[\] | 0\.204 | 1\.60E\-07 | 21286 | 42 | neo\-00011122\-2333\-4445\-5566\-677788899900 | 

## Get started<a name="inference-recommender-neo-compilation-get-started"></a>

The general steps for creating an Inference Recommender job that includes Neo\-optimized recommendations are as follows:
+ Prepare your ML model for compilation\. For more information, see [Prepare Model for Compilation](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-compilation-preparing-model.html) in the Neo documentation\.
+ Package your model in a model archive \(`.tar.gz` file\)\.
+ Create a sample payload archive\.
+ Register your model in Model Registry\.
+ Create an Inference Recommender job\.
+ View the results of the Inference Recommender job and choose a configuration\.
+ Debug compilation failures, if any\. For more information, see [Troubleshoot Neo Compilation Errors](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting-compilation.html)\.

For an example that demonstrates the previous workflow and how to get Neo\-optimized recommendations using XGBoost, see the following [example notebook](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-inference-recommender/xgboost/xgboost-inference-recommender.ipynb)\. For an example that show how to get Neo\-optimized recommendations using TensorFlow, see the following [example notebook](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-inference-recommender/inference-recommender.ipynb)\.