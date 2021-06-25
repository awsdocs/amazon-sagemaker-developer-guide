# Interpret Results<a name="model-monitor-interpreting-results"></a>

After you run a baseline processing job and obtained statistics and constraint for your dataset, you can execute monitoring jobs that calculate statistics and list any violations encountered relative to the baseline constraints\. Amazon CloudWatch metrics are also reported in your account by default\. For information on viewing the results of monitoring in Amazon SageMaker Studio, see [Visualize Results in Amazon SageMaker Studio](model-monitor-interpreting-visualize-results.md)\.

## List Executions<a name="w1854aac29c26c31b5"></a>

The schedule starts monitoring jobs at the specified intervals\. The following code lists the latest five executions\. If you are running this code after creating the hourly schedule, the executions might be empty, and you might have to wait until you cross the hour boundary \(in UTC\) to see the executions start\. The following code includes the logic for waiting\.

```
mon_executions = my_default_monitor.list_executions()
print("We created a hourly schedule above and it will kick off executions ON the hour (plus 0 - 20 min buffer.\nWe will have to wait till we hit the hour...")

while len(mon_executions) == 0:
    print("Waiting for the 1st execution to happen...")
    time.sleep(60)
    mon_executions = my_default_monitor.list_executions()
```

## Inspect a Specific Execution<a name="w1854aac29c26c31b7"></a>

 

In the previous step, you picked up the latest completed or failed scheduled execution\. You can explore what went right or wrong\. The terminal states are:
+ `Completed` – The monitoring execution completed and no issues were found in the violations report\.
+ `CompletedWithViolations` – The execution completed, but constraint violations were detected\.
+ `Failed` – The monitoring execution failed, possibly due to client error \(for example, a role issues\) or infrastructure issues\. To identify the cause, see the `FailureReason` and `ExitMessage`\.

```
latest_execution = mon_executions[-1] # latest execution's index is -1, previous is -2 and so on..
time.sleep(60)
latest_execution.wait(logs=False)

print("Latest execution status: {}".format(latest_execution.describe()['ProcessingJobStatus']))
print("Latest execution result: {}".format(latest_execution.describe()['ExitMessage']))

latest_job = latest_execution.describe()
if (latest_job['ProcessingJobStatus'] != 'Completed'):
        print("====STOP==== \n No completed executions to inspect further. Please wait till an execution completes or investigate previously reported failures.")
```

```
report_uri=latest_execution.output.destination
print('Report Uri: {}'.format(report_uri))
```

## List Generated Reports<a name="w1854aac29c26c31b9"></a>

List the generated reportsUse the following code to list the generated reports\. 

```
from urllib.parse import urlparse
s3uri = urlparse(report_uri)
report_bucket = s3uri.netloc
report_key = s3uri.path.lstrip('/')
print('Report bucket: {}'.format(report_bucket))
print('Report key: {}'.format(report_key))

s3_client = boto3.Session().client('s3')
result = s3_client.list_objects(Bucket=report_bucket, Prefix=report_key)
report_files = [report_file.get("Key") for report_file in result.get('Contents')]
print("Found Report Files:")
print("\n ".join(report_files))
```

## Violations Report<a name="w1854aac29c26c31c11"></a>

If there are violations compared to the baseline, they are generated in the violations report\. Use the following code to list the violations\.

```
violations = my_default_monitor.latest_monitoring_constraint_violations()
pd.set_option('display.max_colwidth', -1)
constraints_df = pd.io.json.json_normalize(violations.body_dict["violations"])
constraints_df.head(10)
```

This applies only to datasets that contain tabular data\. The following schema files specify the statistics calculated and the violations monitored for\.


**Output Files for Tabular Datasets**  

| File Name | Description | 
| --- | --- | 
| statistics\.json |  Contains columnar statistics for each feature in the dataset that is analyzed\. See the schema of this file in the next topic\.  This file is created only for data quality monitoring\.   | 
| constraints\_violations\.json |  Contains a list of violations found in this current set of data as compared to the baseline statistics and constraints file specified in the `baseline_constaints` and `baseline_statistics` paths\.  | 

The [Amazon SageMaker Model Monitor Prebuilt Container](model-monitor-pre-built-container.md) saves a set of Amazon CloudWatch metrics for each feature by default\. 

The container code can emit CloudWatch metrics in this location: `/opt/ml/output/metrics/cloudwatch`\. 