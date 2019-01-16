# Search<a name="API_Search"></a>

Finds Amazon SageMaker resources that match a search query\. Matching resource objects are returned as a list of `SearchResult` objects in the response\. You can sort the search results by any resource property in a ascending or descending order\.

You can query against the following value types: numerical, text, Booleans, and timestamps\.

## Request Syntax<a name="API_Search_RequestSyntax"></a>

```
{
   "[MaxResults](#SageMaker-Search-request-MaxResults)": number,
   "[NextToken](#SageMaker-Search-request-NextToken)": "string",
   "[Resource](#SageMaker-Search-request-Resource)": "string",
   "[SearchExpression](#SageMaker-Search-request-SearchExpression)": { 
      "[Filters](API_SearchExpression.md#SageMaker-Type-SearchExpression-Filters)": [ 
         { 
            "[Name](API_Filter.md#SageMaker-Type-Filter-Name)": "string",
            "[Operator](API_Filter.md#SageMaker-Type-Filter-Operator)": "string",
            "[Value](API_Filter.md#SageMaker-Type-Filter-Value)": "string"
         }
      ],
      "[NestedFilters](API_SearchExpression.md#SageMaker-Type-SearchExpression-NestedFilters)": [ 
         { 
            "[Filters](API_NestedFilters.md#SageMaker-Type-NestedFilters-Filters)": [ 
               { 
                  "[Name](API_Filter.md#SageMaker-Type-Filter-Name)": "string",
                  "[Operator](API_Filter.md#SageMaker-Type-Filter-Operator)": "string",
                  "[Value](API_Filter.md#SageMaker-Type-Filter-Value)": "string"
               }
            ],
            "[NestedPropertyName](API_NestedFilters.md#SageMaker-Type-NestedFilters-NestedPropertyName)": "string"
         }
      ],
      "[Operator](API_SearchExpression.md#SageMaker-Type-SearchExpression-Operator)": "string",
      "[SubExpressions](API_SearchExpression.md#SageMaker-Type-SearchExpression-SubExpressions)": [ 
         "[SearchExpression](API_SearchExpression.md)"
      ]
   },
   "[SortBy](#SageMaker-Search-request-SortBy)": "string",
   "[SortOrder](#SageMaker-Search-request-SortOrder)": "string"
}
```

## Request Parameters<a name="API_Search_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [MaxResults](#API_Search_RequestSyntax) **   <a name="SageMaker-Search-request-MaxResults"></a>
The maximum number of results to return in a `SearchResponse`\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NextToken](#API_Search_RequestSyntax) **   <a name="SageMaker-Search-request-NextToken"></a>
If more than `MaxResults` resource objects match the specified `SearchExpression`, the `SearchResponse` includes a `NextToken`\. The `NextToken` can be passed to the next `SearchRequest` to continue retrieving results for the specified `SearchExpression` and `Sort` parameters\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [Resource](#API_Search_RequestSyntax) **   <a name="SageMaker-Search-request-Resource"></a>
The name of the Amazon SageMaker resource to search for\. Currently, the only valid `Resource` value is `TrainingJob`\.  
Type: String  
Valid Values:` TrainingJob`   
Required: Yes

 ** [SearchExpression](#API_Search_RequestSyntax) **   <a name="SageMaker-Search-request-SearchExpression"></a>
A Boolean conditional statement\. Resource objects must satisfy this condition to be included in search results\. You must provide at least one subexpression, filter, or nested filter\. The maximum number of recursive `SubExpressions`, `NestedFilters`, and `Filters` that can be included in a `SearchExpression` object is 50\.  
Type: [SearchExpression](API_SearchExpression.md) object  
Required: No

 ** [SortBy](#API_Search_RequestSyntax) **   <a name="SageMaker-Search-request-SortBy"></a>
The name of the resource property used to sort the `SearchResults`\. The default is `LastModifiedTime`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Required: No

 ** [SortOrder](#API_Search_RequestSyntax) **   <a name="SageMaker-Search-request-SortOrder"></a>
How `SearchResults` are ordered\. Valid values are `Ascending` or `Descending`\. The default is `Descending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

## Response Syntax<a name="API_Search_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-Search-response-NextToken)": "string",
   "[Results](#SageMaker-Search-response-Results)": [ 
      { 
         "[TrainingJob](API_SearchRecord.md#SageMaker-Type-SearchRecord-TrainingJob)": { 
            "[AlgorithmSpecification](API_TrainingJob.md#SageMaker-Type-TrainingJob-AlgorithmSpecification)": { 
               "[AlgorithmName](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-AlgorithmName)": "string",
               "[MetricDefinitions](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-MetricDefinitions)": [ 
                  { 
                     "[Name](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Name)": "string",
                     "[Regex](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Regex)": "string"
                  }
               ],
               "[TrainingImage](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-TrainingImage)": "string",
               "[TrainingInputMode](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-TrainingInputMode)": "string"
            },
            "[CreationTime](API_TrainingJob.md#SageMaker-Type-TrainingJob-CreationTime)": number,
            "[EnableNetworkIsolation](API_TrainingJob.md#SageMaker-Type-TrainingJob-EnableNetworkIsolation)": boolean,
            "[FailureReason](API_TrainingJob.md#SageMaker-Type-TrainingJob-FailureReason)": "string",
            "[FinalMetricDataList](API_TrainingJob.md#SageMaker-Type-TrainingJob-FinalMetricDataList)": [ 
               { 
                  "[MetricName](API_MetricData.md#SageMaker-Type-MetricData-MetricName)": "string",
                  "[Timestamp](API_MetricData.md#SageMaker-Type-MetricData-Timestamp)": number,
                  "[Value](API_MetricData.md#SageMaker-Type-MetricData-Value)": number
               }
            ],
            "[HyperParameters](API_TrainingJob.md#SageMaker-Type-TrainingJob-HyperParameters)": { 
               "string" : "string" 
            },
            "[InputDataConfig](API_TrainingJob.md#SageMaker-Type-TrainingJob-InputDataConfig)": [ 
               { 
                  "[ChannelName](API_Channel.md#SageMaker-Type-Channel-ChannelName)": "string",
                  "[CompressionType](API_Channel.md#SageMaker-Type-Channel-CompressionType)": "string",
                  "[ContentType](API_Channel.md#SageMaker-Type-Channel-ContentType)": "string",
                  "[DataSource](API_Channel.md#SageMaker-Type-Channel-DataSource)": { 
                     "[S3DataSource](API_DataSource.md#SageMaker-Type-DataSource-S3DataSource)": { 
                        "[AttributeNames](API_S3DataSource.md#SageMaker-Type-S3DataSource-AttributeNames)": [ "string" ],
                        "[S3DataDistributionType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataDistributionType)": "string",
                        "[S3DataType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataType)": "string",
                        "[S3Uri](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3Uri)": "string"
                     }
                  },
                  "[InputMode](API_Channel.md#SageMaker-Type-Channel-InputMode)": "string",
                  "[RecordWrapperType](API_Channel.md#SageMaker-Type-Channel-RecordWrapperType)": "string",
                  "[ShuffleConfig](API_Channel.md#SageMaker-Type-Channel-ShuffleConfig)": { 
                     "[Seed](API_ShuffleConfig.md#SageMaker-Type-ShuffleConfig-Seed)": number
                  }
               }
            ],
            "[LabelingJobArn](API_TrainingJob.md#SageMaker-Type-TrainingJob-LabelingJobArn)": "string",
            "[LastModifiedTime](API_TrainingJob.md#SageMaker-Type-TrainingJob-LastModifiedTime)": number,
            "[ModelArtifacts](API_TrainingJob.md#SageMaker-Type-TrainingJob-ModelArtifacts)": { 
               "[S3ModelArtifacts](API_ModelArtifacts.md#SageMaker-Type-ModelArtifacts-S3ModelArtifacts)": "string"
            },
            "[OutputDataConfig](API_TrainingJob.md#SageMaker-Type-TrainingJob-OutputDataConfig)": { 
               "[KmsKeyId](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-KmsKeyId)": "string",
               "[S3OutputPath](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-S3OutputPath)": "string"
            },
            "[ResourceConfig](API_TrainingJob.md#SageMaker-Type-TrainingJob-ResourceConfig)": { 
               "[InstanceCount](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceCount)": number,
               "[InstanceType](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceType)": "string",
               "[VolumeKmsKeyId](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeKmsKeyId)": "string",
               "[VolumeSizeInGB](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeSizeInGB)": number
            },
            "[RoleArn](API_TrainingJob.md#SageMaker-Type-TrainingJob-RoleArn)": "string",
            "[SecondaryStatus](API_TrainingJob.md#SageMaker-Type-TrainingJob-SecondaryStatus)": "string",
            "[SecondaryStatusTransitions](API_TrainingJob.md#SageMaker-Type-TrainingJob-SecondaryStatusTransitions)": [ 
               { 
                  "[EndTime](API_SecondaryStatusTransition.md#SageMaker-Type-SecondaryStatusTransition-EndTime)": number,
                  "[StartTime](API_SecondaryStatusTransition.md#SageMaker-Type-SecondaryStatusTransition-StartTime)": number,
                  "[Status](API_SecondaryStatusTransition.md#SageMaker-Type-SecondaryStatusTransition-Status)": "string",
                  "[StatusMessage](API_SecondaryStatusTransition.md#SageMaker-Type-SecondaryStatusTransition-StatusMessage)": "string"
               }
            ],
            "[StoppingCondition](API_TrainingJob.md#SageMaker-Type-TrainingJob-StoppingCondition)": { 
               "[MaxRuntimeInSeconds](API_StoppingCondition.md#SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds)": number
            },
            "[Tags](API_TrainingJob.md#SageMaker-Type-TrainingJob-Tags)": [ 
               { 
                  "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
                  "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
               }
            ],
            "[TrainingEndTime](API_TrainingJob.md#SageMaker-Type-TrainingJob-TrainingEndTime)": number,
            "[TrainingJobArn](API_TrainingJob.md#SageMaker-Type-TrainingJob-TrainingJobArn)": "string",
            "[TrainingJobName](API_TrainingJob.md#SageMaker-Type-TrainingJob-TrainingJobName)": "string",
            "[TrainingJobStatus](API_TrainingJob.md#SageMaker-Type-TrainingJob-TrainingJobStatus)": "string",
            "[TrainingStartTime](API_TrainingJob.md#SageMaker-Type-TrainingJob-TrainingStartTime)": number,
            "[TuningJobArn](API_TrainingJob.md#SageMaker-Type-TrainingJob-TuningJobArn)": "string",
            "[VpcConfig](API_TrainingJob.md#SageMaker-Type-TrainingJob-VpcConfig)": { 
               "[SecurityGroupIds](API_VpcConfig.md#SageMaker-Type-VpcConfig-SecurityGroupIds)": [ "string" ],
               "[Subnets](API_VpcConfig.md#SageMaker-Type-VpcConfig-Subnets)": [ "string" ]
            }
         }
      }
   ]
}
```

## Response Elements<a name="API_Search_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_Search_ResponseSyntax) **   <a name="SageMaker-Search-response-NextToken"></a>
If the result of the previous `Search` request was truncated, the response includes a NextToken\. To retrieve the next set of results, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.

 ** [Results](#API_Search_ResponseSyntax) **   <a name="SageMaker-Search-response-Results"></a>
A list of `SearchResult` objects\.  
Type: Array of [SearchRecord](API_SearchRecord.md) objects

## Errors<a name="API_Search_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_Search_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/Search) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/Search) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/Search) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/Search) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/Search) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/Search) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/Search) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/Search) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/Search) 