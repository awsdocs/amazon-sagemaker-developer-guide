# Container Contract Inputs<a name="model-monitor-byoc-contract-inputs"></a>

The Amazon SageMaker Model Monitor platform invokes your container code according to a specified schedule\. If you choose to write your own container code, the following environment variables are available\. In this context, you can analyze the current dataset or evaluate the constraints if you choose and emit metrics, if applicable\.

The available environment variables are the same for real\-time endpoints and batch transform jobs, except for the `dataset_format` variable\. If you are using a real\-time endpoint, the `dataset_format` variable supports the following options:

```
{\"sagemakerCaptureJson\": {\"captureIndexNames\": [\"endpointInput\",\"endpointOutput\"]}}
```

If you are using a batch transform job, the `dataset_format` supports the following options:

```
{\"csv\": {\"header\": [\"true\",\"false\"]}}
```

```
{\"json\": {\"line\": [\"true\",\"false\"]}}
```

```
{\"parquet\": {}}
```

The following code sample shows the complete set of environment variables available for your container code \(and uses the `dataset_format` format for a real\-time endpoint\)\.

```
"Environment": {
 "dataset_format": "{\"sagemakerCaptureJson\": {\"captureIndexNames\": [\"endpointInput\",\"endpointOutput\"]}}",
 "dataset_source": "/opt/ml/processing/endpointdata",
 "end_time": "2019-12-01T16: 20: 00Z",
 "output_path": "/opt/ml/processing/resultdata",
 "publish_cloudwatch_metrics": "Disabled",
 "sagemaker_endpoint_name": "endpoint-name",
 "sagemaker_monitoring_schedule_name": "schedule-name",
 "start_time": "2019-12-01T15: 20: 00Z"
}
```


**Parameters**  

| Parameter Name | Description | 
| --- | --- | 
| dataset\_format |  For a job started from a `MonitoringSchedule` backed by an `Endpoint`, this is `sageMakerCaptureJson` with the capture indices `endpointInput`,or `endpointOutput`, or both\. For a batch transform job, this specifies the data format, whether CSV, JSON, or Parquet\.  | 
| dataset\_source |  If you are using a real\-time endpoint, the local path in which the data corresponding to the monitoring period, as specified by `start_time` and `end_time`, are available\. At this path, the data is available in` /{endpoint-name}/{variant-name}/yyyy/mm/dd/hh`\. We sometimes download more than what is specified by the start and end times\. It is up to the container code to parse the data as required\.  | 
| output\_path |  The local path to write output reports and other files\. You specify this parameter in the `CreateMonitoringSchedule` request as `MonitoringOutputConfig.MonitoringOutput[0].LocalPath`\. It is uploaded to the `S3Uri` path specified in `MonitoringOutputConfig.MonitoringOutput[0].S3Uri`\.  | 
| publish\_cloudwatch\_metrics |  For a job launched by `CreateMonitoringSchedule`, this parameter is set to `Enabled`\. The container can choose to write the Amazon CloudWatch output file at `[filepath]`\.  | 
| sagemaker\_endpoint\_name |  If you are using a real\-time endpoint, the name of the `Endpoint` that this scheduled job was launched for\.  | 
| sagemaker\_monitoring\_schedule\_name |  The name of the `MonitoringSchedule` that launched this job\.  | 
| \*sagemaker\_endpoint\_datacapture\_prefix\* |  If you are using a real\-time endpoint, the prefix specified in the `DataCaptureConfig` parameter of the `Endpoint`\. The container can use this if it needs to directly access more data than already downloaded by SageMaker at the `dataset_source` path\.  | 
| start\_time, end\_time |  The time window for this analysis run\. For example, for a job scheduled to run at 05:00 UTC and a job that runs on 20/02/2020, `start_time`: is 2020\-02\-19T06:00:00Z and `end_time`: is 2020\-02\-20T05:00:00Z  | 
| baseline\_constraints: |  The local path of the baseline constraint file specified in` BaselineConfig.ConstraintResource.S3Uri`\. This is available only if this parameter was specified in the `CreateMonitoringSchedule` request\.  | 
| baseline\_statistics |  The local path to the baseline statistics file specified in `BaselineConfig.StatisticsResource.S3Uri`\. This is available only if this parameter was specified in the `CreateMonitoringSchedule` request\.:   | 