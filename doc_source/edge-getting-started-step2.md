# Train, Compile, and Package Your Model<a name="edge-getting-started-step2"></a>

In this section you will create SageMaker and AWS IoT client objects, download a pre\-trained machine learning model, upload your model to your Amazon S3 bucket, compile your model for your target device with SageMaker Neo, and package your model so that it can be deployed with the Edge Manager agent\.

1. **Import libraries and create client objects\.**

   This tutorial uses the AWS SDK for Python \(Boto3\) to create clients to interact with SageMaker, Amazon S3, and AWS IoT\.

   Import Boto3, specify your Region, and initialize the client objects you need as shown in the following example:

   ```
   import boto3
   import json
   import time
   
   AWS_REGION = 'us-west-2'# Specify your Region
   bucket = 'bucket-name'
   
   sagemaker_client = boto3.client('sagemaker', region_name=AWS_REGION)
   iot_client = boto3.client('iot', region_name=AWS_REGION)
   ```

   Define variables and assign them the role ARN you created for SageMaker and AWS IoT as strings:

   ```
   # Replace with the role ARN you created for SageMaker
   sagemaker_role_arn = "arn:aws:iam::<account>:role/*"
   
   # Replace with the role ARN you created for AWS IoT. 
   # Note: The name must start with 'SageMaker'
   iot_role_arn = "arn:aws:iam::<account>:role/SageMaker*"
   ```

1. **Train a machine learning model\.**

   See [Train a Model with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html) for more information on how to train a machine learning model using SageMaker\. You can optionally upload your locally trained model directly into an Amazon S3 URI bucket\.

   If you do not have a model yet, you can use a pre\-trained model for the next steps in this tutorial\. For example, you can save the MobileNet V2 models from the TensorFlow framework\. MobileNet V2 is an image classification model optimized for mobile applications\. For more information about MobileNet V2, see the [MobileNet GitHub README](https://github.com/tensorflow/models/tree/master/research/slim/nets/mobilenet)\.

   Type the following into your Jupyter Notebook to save the pre\-trained MobileNet V2 model:

   ```
   # Save the MobileNet V2 model to local storage
      import tensorflow as tf
      model = tf.keras.applications.MobileNetV2()
      model.save(“mobilenet_v2.h5”)
   ```
**Note**  
If you do not have TensorFlow installed, you can do so by running `pip install tensorflow=2.4`
Use TensorFlow version 2\.4 or lower for this tutorial\.

   The model will be saved into the `mobilenet_v2.h5` file\. Before packaging the model, you will need to first compile your model using SageMaker Neo\. See [Supported Frameworks, Devices, Systems, and Architectures](neo-supported-devices-edge.md) to check if your version of TensorFlow \(or other framework of choice\) is currently supported by SageMaker Neo\.

   SageMaker Neo requires models to be stored as a compressed TAR file\. Repackage it as a compressed TAR file \(\*\.tar\.gz\):

   ```
   # Package MobileNet V2 model into a TAR file 
      import tarfile
      
      tarfile_name='mobilenet-v2.tar.gz'
      
      with tarfile.open(tarfile_name, mode='w:gz') as archive:
          archive.add('mobilenet-v2.h5')
   ```

1. **Upload your model to Amazon S3\.**

   Once you have a machine learning model, store it in an Amazon S3 bucket\. The following example uses an AWS CLI command to upload the model the to the Amazon S3 bucket you created earlier in a directory called *models*\. Type in the following into your Jupyter Notebook:

   ```
   !aws s3 cp mobilenet-v2.tar.gz s3://{bucket}/models/
   ```

1. **Compile your model with SageMaker Neo\.**

   Compile your machine learning model with SageMaker Neo for an edge device\. You need to know your Amazon S3 bucket URI where you stored the trained model, the machine learning framework you used to train your model, the shape of your model’s input, and your target device\.

   For the MobileNet V2 model, use the following:

   ```
   framework = 'tensorflow'
   target_device = 'jetson_nano'
   data_shape = '{"data":[1,3,224,224]}'
   ```

   SageMaker Neo requires a specific model input shape and model format based on the deep learning framework you use\. For more information about how to save your model, see [What input data shapes does SageMaker Neo expect?](neo-compilation-preparing-model.md#neo-job-compilation-expected-inputs)\. For more information about devices and frameworks supported by Neo, see [Supported Frameworks, Devices, Systems, and Architectures](neo-supported-devices-edge.md)\.

   Use the `CreateCompilationJob` API to create a compilation job with SageMaker Neo\. Provide a name to the compilation job, the SageMaker Role ARN, the Amazon S3 URI where your model is stored, the input shape of the model, the name of the framework, the Amazon S3 URI where you want SageMaker to store your compiled model, and your edge device target\.

   ```
   # Specify the path where your model is stored
   model_directory = 'models'
   s3_model_uri = 's3://{}/{}/{}'.format(bucket, model_directory, tarfile_name)
   
   # Store compiled model in S3 within the 'compiled-models' directory
   compilation_output_dir = 'compiled-models'
   s3_output_location = 's3://{}/{}/'.format(bucket, compilation_output_dir)
   
   # Give your compilation job a name
   compilation_job_name = 'getting-started-demo'
   
   sagemaker_client.create_compilation_job(CompilationJobName=compilation_job_name,
                                           RoleArn=sagemaker_role_arn,
                                           InputConfig={
                                               'S3Uri': s3_model_uri,
                                               'DataInputConfig': data_shape,
                                               'Framework' : framework.upper()},
                                           OutputConfig={
                                               'S3OutputLocation': s3_output_location,
                                               'TargetDevice': target_device},
                                           StoppingCondition={'MaxRuntimeInSeconds': 900})
   ```

1. **Package your compiled model\.**

   Packaging jobs take SageMaker Neo–compiled models and make any changes necessary to deploy the model with the inference engine, Edge Manager agent\. To package your model, create an edge packaging job with the `create_edge_packaging` API or the SageMaker console\.

   You need to provide the name that you used for your Neo compilation job, a name for the packaging job, a role ARN \(see [Setting Up](edge-getting-started-step1.md) section\), a name for the model, a model version, and the Amazon S3 bucket URI for the output of the packaging job\. Note that Edge Manager packaging job names are case\-sensitive\. The following is an example of how to create a packaging job using the API\.

   ```
   edge_packaging_name='edge-packaging-demo'
   model_name="sample-model"
   model_version="1.1"
   ```

   Define the Amazon S3 URI where you want to store the packaged model\.

   ```
   # Output directory where you want to store the output of the packaging job
   packaging_output_dir = 'packaged_models'
   packaging_s3_output = 's3://{}/{}'.format(bucket, packaging_output_dir)
   ```

   Use `CreateEdgePackagingJob` to package your Neo\-compiled model\. Provide a name for your edge packaging job and the name you provided for your compilation job \(in this example, it was stored in the variable `compilation_job_name`\)\. Also provide a name for your model, a version for your model \(this is used to help you keep track of what model version you are using\), and the S3 URI where you want SageMaker to store the packaged model\.

   ```
   sagemaker_client.create_edge_packaging_job(
                       EdgePackagingJobName=edge_packaging_name,
                       CompilationJobName=compilation_job_name,
                       RoleArn=sagemaker_role_arn,
                       ModelName=model_name,
                       ModelVersion=model_version,
                       OutputConfig={
                           "S3OutputLocation": packaging_s3_output
                           }
                       )
   ```