# NestedFilters<a name="API_NestedFilters"></a>

Defines a list of `NestedFilters` objects\. To satisfy the conditions specified in the `NestedFilters` call, a resource must satisfy the conditions of all of the filters\.

For example, you could define a `NestedFilters` using the training job's `InputDataConfig` property to filter on `Channel` objects\. 

A `NestedFilters` object contains multiple filters\. For example, to find all training jobs whose name contains `train` and that have `cat/data` in their `S3Uri` \(specified in `InputDataConfig`\), you need to create a `NestedFilters` object that specifies the `InputDataConfig` property with the following `Filter` objects:
+  `'{Name:"InputDataConfig.ChannelName", "Operator":"EQUALS", "Value":"train"}',` 
+  `'{Name:"InputDataConfig.DataSource.S3DataSource.S3Uri", "Operator":"CONTAINS", "Value":"cat/data"}'` 

## Contents<a name="API_NestedFilters_Contents"></a>

 **Filters**   <a name="SageMaker-Type-NestedFilters-Filters"></a>
A list of filters\. Each filter acts on a property\. Filters must contain at least one `Filters` value\. For example, a `NestedFilters` call might include a filter on the `PropertyName` parameter of the `InputDataConfig` property: `InputDataConfig.DataSource.S3DataSource.S3Uri`\.  
Type: Array of [Filter](API_Filter.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 20 items\.  
Required: Yes

 **NestedPropertyName**   <a name="SageMaker-Type-NestedFilters-NestedPropertyName"></a>
The name of the property to use in the nested filters\. The value must match a listed property name, such as `InputDataConfig` \.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.+`   
Required: Yes

## See Also<a name="API_NestedFilters_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/NestedFilters) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/NestedFilters) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/NestedFilters) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/NestedFilters) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/NestedFilters) 