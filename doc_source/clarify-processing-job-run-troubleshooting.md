# Troubleshoot SageMaker Clarify Processing Jobs<a name="clarify-processing-job-run-troubleshooting"></a>

 If you encounter failures with SageMaker Clarify processing jobs, consult the following scenarios to help identify the issue\.

**Note**  
The failure reason and exit message are intended to contain descriptive messages and exceptions, if encountered, during the run\. One common reason is invalid or missing parameters\. If you encounter unclear, confusing, or misleading messages or are unable to find a solution, submit feedback\.

**Topics**
+ [Processing job fails to finish](#clarify-troubleshooting-job-fails)
+ [Processing job finishes without results and you get a CloudWatch warning message](#clarify-troubleshooting-no-results-and-warning)
+ [Error message for invalid analysis configuration](#clarify-troubleshooting-invalid-analysis-configuration)
+ [Bias metric computation fails for several or all metrics](#clarify-troubleshooting-bias-metric-computation-fails)
+ [Mismatch between analysis config and dataset/model input/output](#clarify-troubleshooting-mismatch-analysis-config-and-data-model)
+ [Model returns 500 Internal Server Error or container falls back to per\-record predictions due to model error](#clarify-troubleshooting-500-internal-server-error)
+ [Execution role is invalid](#clarify-troubleshooting-execution-role-invalid)
+ [Failed to download data](#clarify-troubleshooting-TBD)
+ [Could not connect to SageMaker](#clarify-troubleshooting-TBD)

## Processing job fails to finish<a name="clarify-troubleshooting-job-fails"></a>

If the processing job fails to finish, you can try the following:
+ Inspect the job logs directly in the notebook where you ran the job in\. The job logs are located in the output of the notebook cell where you initiated the run\.
+ Inspect the job logs in CloudWatch\.
+ Add the following line in your notebook to describe the last processing job and look for the failure reason and exit message:
  + `clarify_processor.jobs[-1].describe()`
+ Execute the following AWS CLI command to describe the processing job and look for the failure reason and exit message:
  + `aws sagemaker describe-processing-job —processing-job-name <processing-job-id>`

## Processing job finishes without results and you get a CloudWatch warning message<a name="clarify-troubleshooting-no-results-and-warning"></a>

If the processing job finishes but no results are found and a warning message is found in the CloudWatch logs that says “Signal 15 received, cleaning up”, this is an indication that the job was stopped either due to a customer request that called the `StopProcessingJob` API or that the job ran out of time allotted for its completion\. In the later case, check the maximum runtime in the job configuration \(`max_runtime_in_seconds`\) and increase it as needed\.

## Error message for invalid analysis configuration<a name="clarify-troubleshooting-invalid-analysis-configuration"></a>
+  If you get the error message "Unable to load analysis configuration as JSON\.", this means that the analysis configuration input file for the processing job does not contain a valid JSON object\. Check the validity of the JSON object using a JSON linter\.
+ If you get the error message "Analysis configuration schema validation error\.", this means that the analysis configuration input file for the processing job contains unknown fields or invalid types for some field values\. Review the configuration parameters in the file and cross\-check them with the parameters listed in the [configuration specification](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-configure-processing-jobs.html#clarify-processing-job-configure-analysis) file\.

## Bias metric computation fails for several or all metrics<a name="clarify-troubleshooting-bias-metric-computation-fails"></a>

If your receive one of the following error messages "No Label values are present in the predicted Label Column, Positive Predicted Index Series contains all False values\." or "Predicted Label Column series data type is not the same as Label Column series\.", try the following:
+ Check that the correct dataset is being used\.
+ Check whether the dataset size is too small; whether, for example, it contains only a few rows\. This may cause the model outputs to have the same value or the data type is inferred incorrectly\.
+ Check if the label or facet is treated as continuous or categorical\. SageMaker Clarify uses heuristics to determine the [https://github.com/aws/amazon-sagemaker-clarify/blob/master/src/smclarify/bias/metrics/common.py#L114)](https://github.com/aws/amazon-sagemaker-clarify/blob/master/src/smclarify/bias/metrics/common.py#L114))\. For post\-training bias metrics, the data type returned by the model may not match what is in the dataset or SageMaker Clarify may not be able to transform it correctly\. 
  + In the bias report, you should see a single value for categorical columns or an interval for continuous columns\.
  + For example, if a column has values 0\.0 and 1\.0 as floats, it will be treated as continuous even if there are too few unique values\.

## Mismatch between analysis config and dataset/model input/output<a name="clarify-troubleshooting-mismatch-analysis-config-and-data-model"></a>
+ Check that the baseline format in the analysis config is the same as dataset format\.
+ If your receive the error message "Could not convert string to float\.", check that the format is correctly specified\. It could also indicate that the model predictions have a different format than the label column or it could indicate that the configuration for the label or probabilities is incorrect\.
+ If your receive the error message "Unable to locate the facet\." or "Headers must contain label\." or "Headers in config do not match with the number of columns in the dataset\." or "Feature names not found\.", check that the headers match the columns\.
+ If your receive the error message "Data must contain features\.", check the content template for JSON Lines and compare it with the dataset sample if available\.

## Model returns 500 Internal Server Error or container falls back to per\-record predictions due to model error<a name="clarify-troubleshooting-500-internal-server-error"></a>

If you receive the error message "Fallback to per\-record prediction because of model error\.", this could indicate that model cannot handle the batch size, or be throttled, or just does not accept the input passed by the container due to serialization problems\. You should review the CloudWatch logs for the SageMaker endpoint and look for error messages or tracebacks\. For model throttling cases, it may help to use a different instance type or increasing the number of instances for the endpoint\.

## Execution role is invalid<a name="clarify-troubleshooting-execution-role-invalid"></a>

This indicates that the role provided is incorrect or missing required permissions\. Check the role and its permissions that were used to configure the processing job and verify the permission and trust policy for the role\.

## Failed to download data<a name="clarify-troubleshooting-TBD"></a>

This indicates that job inputs could not be downloaded for the job to start\. Check the bucket name and permissions for the dataset and the configuration inputs\.

## Could not connect to SageMaker<a name="clarify-troubleshooting-TBD"></a>

This indicates that the job could not reach SageMaker service endpoints\. Check the network configuration settings for the processing job and verify VPC configuration\.