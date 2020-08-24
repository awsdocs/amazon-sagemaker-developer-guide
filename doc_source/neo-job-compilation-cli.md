# Compile a Model \(AWS Command Line Interface\)<a name="neo-job-compilation-cli"></a>

This section shows how to manage Neo compilation jobs for machine learning models using AWS Command Line Interface \(CLI\)\. You can create, describe, stop, and list the compilation jobs\. 

1. Create a Compilation Job

   With the [CreateCompilationJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html) API operation, you can specify the data input format, the S3 bucket to store your model, the S3 bucket to write the compiled model, and the target hardware device or platform\.
   + For a target device:

     ```
     {
         "CompilationJobName": "neo-compilation-job-demo",
         "RoleArn": "arn:aws:iam::<your-account>:role/service-role/AmazonSageMaker-ExecutionRole-yyyymmddThhmmss",
         "InputConfig": {
             "S3Uri": "s3://<your-bucket>/sagemaker/neo-compilation-job-demo-data/train",
             "DataInputConfig":  "{'data': [1,3,1024,1024]}",
             "Framework": "MXNET"
         },
         "OutputConfig": {
             "S3OutputLocation": "s3://<your-bucket>/sagemaker/neo-compilation-job-demo-data/compile",
             # A target device specification example for a ml_c5 instance family
             "TargetDevice": "ml_c5"
         },
         "StoppingCondition": {
             "MaxRuntimeInSeconds": 300
         }
     }
     ```
   + For a target platform:

     ```
     {
         "CompilationJobName": "neo-test-compilation-job",
         "RoleArn": "arn:aws:iam::<your-account>:role/service-role/AmazonSageMaker-ExecutionRole-yyyymmddThhmmss",
         "InputConfig": {
             "S3Uri": "s3://<your-bucket>/sagemaker/neo-compilation-job-demo-data/train",
             "DataInputConfig":  "{'data': [1,3,1024,1024]}",
             "Framework": "MXNET"
         },
         "OutputConfig": {
             "S3OutputLocation": "s3://<your-bucket>/sagemaker/neo-compilation-job-demo-data/compile",
             # A target platform configuration example for a p3.2xlarge instance
             "TargetPlatform": {
                 "Os": "LINUX",
                 "Arch": "X86_64",
                 "Accelerator": "NVIDIA"
             },
             "CompilerOptions": "{'cuda-ver': '10.0', 'trt-ver': '6.0.1', 'gpu-code': 'sm_70'}"
         },
         "StoppingCondition": {
             "MaxRuntimeInSeconds": 300
         }
     }
     ```
**Note**  
For the `OutputConfig` API operation, the `TargetDevice` and `TargetPlatform` API operations are mutually exclusive\. You have to choose one between the two options\.

   To find the JSON string examples of `DataInputConfig` depending on frameworks, see [What input data shapes Neo expects](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting.html#neo-troubleshooting-errors-preventing)\.

   For more information about setting up the configurations, see the [InputConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InputConfig.html), [OutputConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OutputConfig.html), and [TargetPlatform](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TargetPlatform.html) API operations in the SageMaker API reference\.

1. After you configure the JSON file, run the following command to create the compilation job:

   ```
   aws sagemaker create-compilation-job \
   --cli-input-json file://job.json \
   --region us-west-2 
   
   # You should get CompilationJobArn
   ```

1. Describe the compilation job by running the following command:

   ```
   aws sagemaker describe-compilation-job \
   --compilation-job-name $JOB_NM \
   --region us-west-2
   ```

1. Stop the compilation job by running the following command:

   ```
   aws sagemaker stop-compilation-job \
   --compilation-job-name $JOB_NM \
   --region us-west-2
   
   # There is no output for compilation-job operation
   ```

1. List the compilation job by running the following command:

   ```
   aws sagemaker list-compilation-jobs \
   --region us-west-2
   ```