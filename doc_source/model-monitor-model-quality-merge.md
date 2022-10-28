# Ingest Ground Truth Labels and Merge Them With Predictions<a name="model-monitor-model-quality-merge"></a>

Model quality monitoring compares the predictions your model makes with ground truth labels to measure the quality of the model\. For this to work, you periodically label data captured by your endpoint or batch transform job and upload it to Amazon S3\.

To match Ground Truth labels with captured prediction data, there must be a unique identifier for each record in the dataset\. The structure of each record for ground truth data is as follows:

```
{
  "groundTruthData": {
    "data": "1",
    "encoding": "CSV" # only CSV supported at launch, we assume "data" only consists of label
  },
  "eventMetadata": {
    "eventId": "aaaa-bbbb-cccc"
  },
  "eventVersion": "0"
}
```

In the `groundTruthData` structure, `eventId` can be one of the following:
+ `eventId` – This ID is automatically generated when a user invokes the endpoint\.
+ `inferenceId` – The caller supplies this ID when they invoke the endpoint\.

If `inferenceId` is present in captured data records, Model Monitor uses it to merge captured data with Ground Truth records\. You are responsible for making sure that the `inferenceId` in the Ground Truth records match the `inferenceId` in the captured records\. If `inferenceId` is not present in captured data, model monitor uses `eventId` from the captured data records to match them with a Ground Truth record\.

You must upload Ground Truth data to an Amazon S3 bucket that has the same path format as captured data, which is of the following form:

```
s3://bucket/prefix/yyyy/mm/dd/hh
```

The date in this path is the date when the Ground Truth label is collected, and does not have to match the date when the inference was generated\.

After you create and upload the Ground Truth labels, include the location of the labels as a parameter when you create the monitoring job\. If you are using AWS SDK for Python \(Boto3\), do this by specifying the location of Ground Truth labels as the `S3Uri` field of the `GroundTruthS3Input` parameter in a call to the `create_model_quality_job_definition` method\. If you are using the SageMaker Python SDK, specify the location of the Ground Truth labels as the `ground_truth_input` parameter in the call to the `create_monitoring_schedule` of the `ModelQualityMonitor` object\.