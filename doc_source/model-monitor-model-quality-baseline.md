# Create a Model Quality Baseline<a name="model-monitor-model-quality-baseline"></a>

Create a baseline job that compares your model predictions with ground truth labels in a baseline dataset that you have stored in Amazon S3\. Typically, you use a training dataset as the baseline dataset\. The baseline job calculates metrics for the model and suggests constraints to use to monitor model quality drift\.

To create a baseline job, you need to have a dataset that contains predictions from your model along with labels that represent the Ground Truth for your data\.

To create a baseline job use the `ModelQualityMonitor` class provided by the SageMaker Python SDK, and complete the following steps\.

**To create a model quality baseline job**

1.  First, create an instance of the `ModelQualityMonitor` class\. The following code snippet shows how to do this\.

   ```
   from sagemaker import get_execution_role, session, Session
   from sagemaker.model_monitor import ModelQualityMonitor
                   
   role = get_execution_role()
   session = Session()
   
   model_quality_monitor = ModelQualityMonitor(
       role=role,
       instance_count=1,
       instance_type='ml.m5.xlarge',
       volume_size_in_gb=20,
       max_runtime_in_seconds=1800,
       sagemaker_session=session
   )
   ```

1. Now call the `suggest_baseline` method of the `ModelQualityMonitor` object to run a baseline job\. The following code snippet assumes that you have a baseline dataset that contains both predictions and labels stored in Amazon S3\.

   ```
   baseline_job_name = "MyBaseLineJob"
   job = model_quality_monitor.suggest_baseline(
       job_name=baseline_job_name,
       baseline_dataset=baseline_dataset_uri, # The S3 location of the validation dataset.
       dataset_format=DatasetFormat.csv(header=True),
       output_s3_uri = baseline_results_uri, # The S3 location to store the results.
       problem_type='BinaryClassification',
       inference_attribute= "prediction", # The column in the dataset that contains predictions.
       probability_attribute= "probability", # The column in the dataset that contains probabilities.
       ground_truth_attribute= "label" # The column in the dataset that contains ground truth labels.
   )
   job.wait(logs=False)
   ```

1. After the baseline job finishes, you can see the constraints that the job generated\. First, get the results of the baseline job by calling the `latest_baselining_job` method of the `ModelQualityMonitor` object\.

   ```
   baseline_job = model_quality_monitor.latest_baselining_job
   ```

1. The baseline job suggests constraints, which are thresholds for metrics that model monitor measures\. If a metric goes beyond the suggested threshold, Model Monitor reports a violation\. To view the constraints that the baseline job generated, call the `suggested_constraints` method of the baseline job\. The following code snippet loads the constraints for a binary classification model into a Pandas dataframe\.

   ```
   import pandas as pd
   pd.DataFrame(baseline_job.suggested_constraints().body_dict["binary_classification_constraints"]).T
   ```

   We recommend that you view the generated constraints and modify them as necessary before using them for monitoring\. For example, if a constraint is too aggressive, you might get more alerts for violations than you want\.

   If your constraint contains numbers expressed in scientific notation, you will need to convert them to float\. The following python [preprocessing script](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-pre-and-post-processing.html#model-monitor-pre-processing-script) example shows how to convert numbers in scientific notation to float\. 

   ```
   import csv
   
   def fix_scientific_notation(col):
       try:
           return format(float(col), "f")
       except:
           return col
   
   def preprocess_handler(csv_line):
       reader = csv.reader([csv_line])
       csv_record = next(reader)
       #skip baseline header, change HEADER_NAME to the first column's name
       if csv_record[0] == “HEADER_NAME”:
          return []
       return { str(i).zfill(20) : fix_scientific_notation(d) for i, d in enumerate(csv_record)}
   ```

   You can add your pre\-processing script to a baseline or monitoring schedule as a `record_preprocessor_script`, as defined in the [Model Monitor](https://sagemaker.readthedocs.io/en/stable/api/inference/model_monitor.html) documentation\.

1. When you are satisfied with the constraints, pass them as the `constraints` parameter when you create a monitoring schedule\. For more information, see [Schedule Model Quality Monitoring Jobs](model-monitor-model-quality-schedule.md)\.

The suggested baseline constraints are contained in the constraints\.json file in the location you specify with `output_s3_uri`\. For information about the schema for this file in the [Schema for Constraints \(constraints\.json file\)](model-monitor-byoc-constraints.md)\.