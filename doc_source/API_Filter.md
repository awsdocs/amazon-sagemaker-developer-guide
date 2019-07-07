# Filter<a name="API_Filter"></a>

A conditional statement for a search expression that includes a Boolean operator, a resource property, and a value\.

If you don't specify an `Operator` and a `Value`, the filter searches for only the specified property\. For example, defining a `Filter` for the `FailureReason` for the `TrainingJob` `Resource` searches for training job objects that have a value in the `FailureReason` field\.

If you specify a `Value`, but not an `Operator`, Amazon SageMaker uses the equals operator as the default\.

In search, there are several property types:

Metrics  
To define a metric filter, enter a value using the form `"Metrics.<name>"`, where `<name>` is a metric name\. For example, the following filter searches for training jobs with an `"accuracy"` metric greater than `"0.9"`:  
 `{`   
 `"Name": "Metrics.accuracy",`   
 `"Operator": "GREATER_THAN",`   
 `"Value": "0.9"`   
 `}` 

HyperParameters  
To define a hyperparameter filter, enter a value with the form `"HyperParameters.<name>"`\. Decimal hyperparameter values are treated as a decimal in a comparison if the specified `Value` is also a decimal value\. If the specified `Value` is an integer, the decimal hyperparameter values are treated as integers\. For example, the following filter is satisfied by training jobs with a `"learning_rate"` hyperparameter that is less than `"0.5"`:  
 ` {`   
 ` "Name": "HyperParameters.learning_rate",`   
 ` "Operator": "LESS_THAN",`   
 ` "Value": "0.5"`   
 ` }` 

Tags  
To define a tag filter, enter a value with the form `"Tags.<key>"`\.

## Contents<a name="API_Filter_Contents"></a>

 **Name**   <a name="SageMaker-Type-Filter-Name"></a>
A property name\. For example, `TrainingJobName`\. For the list of valid property names returned in a search result for each supported resource, see [TrainingJob](API_TrainingJob.md) properties\. You must specify a valid property name for the resource\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.+`   
Required: Yes

 **Operator**   <a name="SageMaker-Type-Filter-Operator"></a>
A Boolean binary operator that is used to evaluate the filter\. The operator field contains one of the following values:    
Equals  
The specified resource in `Name` equals the specified `Value`\.  
NotEquals  
The specified resource in `Name` does not equal the specified `Value`\.  
GreaterThan  
The specified resource in `Name` is greater than the specified `Value`\. Not supported for text\-based properties\.  
GreaterThanOrEqualTo  
The specified resource in `Name` is greater than or equal to the specified `Value`\. Not supported for text\-based properties\.  
LessThan  
The specified resource in `Name` is less than the specified `Value`\. Not supported for text\-based properties\.  
LessThanOrEqualTo  
The specified resource in `Name` is less than or equal to the specified `Value`\. Not supported for text\-based properties\.  
Contains  
Only supported for text\-based properties\. The word\-list of the property contains the specified `Value`\.
If you have specified a filter `Value`, the default is `Equals`\.  
Type: String  
Valid Values:` Equals | NotEquals | GreaterThan | GreaterThanOrEqualTo | LessThan | LessThanOrEqualTo | Contains`   
Required: No

 **Value**   <a name="SageMaker-Type-Filter-Value"></a>
A value used with `Resource` and `Operator` to determine if objects satisfy the filter's condition\. For numerical properties, `Value` must be an integer or floating\-point decimal\. For timestamp properties, `Value` must be an ISO 8601 date\-time string of the following format: `YYYY-mm-dd'T'HH:MM:SS`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `.+`   
Required: No

## See Also<a name="API_Filter_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/Filter) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/Filter) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/Filter) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/Filter) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/Filter) 