# Step 1: Compile the Model<a name="neo-getting-started-edge-step1"></a>

Once you have satisfied the [Prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-getting-started-edge.html#neo-getting-started-edge-step0), you can compile your model with Amazon SageMaker Neo\. You can compile your model using the AWS CLI, the console or the [Amazon Web Services SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html), see [Use Neo to Compile a Model](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-job-compilation.html)\. In this example, you will compile your model with Boto3\.

To compile a model, SageMaker Neo requires the following information:

1.  **The Amazon S3 bucket URI where you stored the trained model\.** 

   If you followed the prerequisites, the name of your bucket is stored in a variable named `bucket`\. The following code snippet shows how to list all of your buckets using the AWS CLI: 

   ```
   aws s3 ls
   ```

   For example: 

   ```
   $ aws s3 ls
   2020-11-02 17:08:50 bucket
   ```

1.  **The Amazon S3 bucket URI where you want to save the compiled model\.** 

   The code snippet below concatenates your Amazon S3 bucket URI with the name of an output directory called `output`: 

   ```
   s3_output_location = f's3://{bucket}/output'
   ```

1.  **The machine learning framework you used to train your model\.** 

   Define the framework you used to train your model\.

   ```
   framework = 'framework-name'
   ```

   For example, if you wanted to compile a model that was trained using TensorFlow, you could either use `tflite` or `tensorflow`\. Use `tflite` if you want to use a lighter version of TensorFlow that uses less storage memory\. 

   ```
   framework = 'tflite'
   ```

   For a complete list of Neo\-supported frameworks, see [Supported Frameworks, Devices, Systems, and Architectures](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-supported-devices-edge.html)\. 

1.  **The shape of your model's input\.** 

    Neo requires the name and shape of your input tensor\. The name and shape are passed in as key\-value pairs\. `value` is a list of the integer dimensions of an input tensor and `key` is the exact name of an input tensor in the model\. 

   ```
   data_shape = '{"name": [tensor-shape]}'
   ```

   For example:

   ```
   data_shape = '{"normalized_input_image_tensor":[1, 300, 300, 3]}'
   ```
**Note**  
Make sure the model is correctly formatted depending on the framework you used\. See [What input data shapes does SageMaker Neo expect?](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-job-compilation.html#neo-job-compilation-expected-inputs) The key in this dictionary must be changed to the new input tensor's name\.

1.  **Either the name of the target device to compile for or the general details of the hardware platform** 

   ```
   target_device = 'target-device-name'
   ```

   For example, if you want to deploy to a Raspberry Pi 3, use: 

   ```
   target_device = 'rasp3b'
   ```

   You can find the entire list of supported edge devices in [Supported Frameworks, Devices, Systems, and Architectures](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-supported-devices-edge.html)\.

 Now that you have completed the previous steps, you can submit a compilation job to Neo\. 

```
# Create a SageMaker client so you can submit a compilation job
sagemaker_client = boto3.client('sagemaker', region_name=AWS_REGION)

# Give your compilation job a name
compilation_job_name = 'getting-started-demo'
print(f'Compilation job for {compilation_job_name} started')

response = sagemaker_client.create_compilation_job(
    CompilationJobName=compilation_job_name,
    RoleArn=role_arn,
    InputConfig={
        'S3Uri': s3_input_location,
        'DataInputConfig': data_shape,
        'Framework': framework.upper()
    },
    OutputConfig={
        'S3OutputLocation': s3_output_location,
        'TargetDevice': target_device 
    },
    StoppingCondition={
        'MaxRuntimeInSeconds': 900
    }
)

# Optional - Poll every 30 sec to check completion status
import time

while True:
    response = sagemaker_client.describe_compilation_job(CompilationJobName=compilation_job_name)
    if response['CompilationJobStatus'] == 'COMPLETED':
        break
    elif response['CompilationJobStatus'] == 'FAILED':
        raise RuntimeError('Compilation failed')
    print('Compiling ...')
    time.sleep(30)
print('Done!')
```

If you want additional information for debugging, include the following print statement:

```
print(response)
```

If the compilation job is successful, your compiled model isstored in the output Amazon S3 bucket you specified earlier \(`s3_output_location`\)\. Download your compiled model locally: 

```
object_path = f'output/{model}-{target_device}.tar.gz'
neo_compiled_model = f'compiled-{model}.tar.gz'
s3_client.download_file(bucket, object_path, neo_compiled_model)
```