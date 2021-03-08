# SageMaker Pipelines Quotas<a name="pipelines-quotas"></a>

The following tables list the quotas for SageMaker Pipelines resources and requirements for pipeline creation\.

## Pipeline Quotas<a name="pipelines-quotas-pipelines"></a>


| Pipeline Property | Quota | 
| --- | --- | 
| Maximum number of pipelines | 5000 | 

## Executions Quotas<a name="pipelines-quotas-executions"></a>


| Execution Property | Quota | 
| --- | --- | 
| Maximum execution time | 28 days | 
| Maximum number of concurrent pipeline executions per account | 200 | 
| Maximum number of concurrent pipeline executions per pipeline | 200 | 

## Step Quotas<a name="pipelines-quotas-steps"></a>


| Step Property | Quota | 
| --- | --- | 
| Maximum number of steps per pipeline | 50 | 
| Maximum step name length | 64 characters | 
| Step name regular expression pattern | \(\[A\-Za\-z0\-9\\\\\-\_\]\)\* | 

## Parameters Quotas<a name="pipelines-quotas-parameters"></a>


| Parameters Property | Quota | 
| --- | --- | 
| Maximum number of parameters per pipeline | 200 | 
| Maximum parameter name length | 64 characters | 
| Maximum parameter name regular expression pattern | \(\[A\-Za\-z0\-9\\\\\-\_\]\)\* | 
| Maximum parameter description length | 4096 characters | 
| Maximum number of parameter enum values | 16 distinct values | 
| Maximum parameter enum value length | 2048 characters | 
| Maximum string default value limit | 2048 characters | 

## Condition Step Quotas<a name="pipelines-quotas-conditions"></a>


| Condition Step Property | Quota | 
| --- | --- | 
| Maximum number of Conditions per ConditionStep | 200 | 
| Maximum number of Steps in If\-List | 20 | 
| Maximum number of Steps in Else\-List | 20 | 
| Maximum number of Conditions in an Or\-list | 200 | 

## Property Files Quotas<a name="pipelines-quotas-propertyfile"></a>


| Property Files Property | Quota | 
| --- | --- | 
| Maximum number of PropertyFiles in a pipeline | 10 | 
| Maximum number of JsonGet functions in a pipeline | 200 | 
| Maximum size of the property file | 2MB | 

## Metadata Quotas<a name="pipelines-quotas-metadata"></a>


| Metadata Property | Quota | 
| --- | --- | 
| Maximum metadata size | 20 key\-value pairs | 
| Maximum metadata key size | 128 characters | 
| Metadata key regular expression pattern | \(\[A\-Za\-z0\-9\\\\\-\_\]\)\* | 
| Maximum metadata value size | 1024 characters | 
| Metadata value regular expression pattern | \[\\\\p\{M\}\\\\p\{L\}\\\\p\{S\}\\\\p\{S\}\\\\p\{N\}\\\\p\{P\}\\\\s\] | 