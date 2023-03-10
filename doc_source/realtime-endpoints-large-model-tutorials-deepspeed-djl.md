# Large model inference with DeepSpeed and DJL Serving<a name="realtime-endpoints-large-model-tutorials-deepspeed-djl"></a>

 This tutorial demonstrates how to deploy large models with DJL Serving using DeepSpeed and Hugging Face Accelerate model parallelization frameworks\. This example uses a GPT\-J model with 6 billion parameters and an `ml.g5` instance\. You can modify this to work with other models and instance types\. Replace the *italicized placeholder text* in the examples with your own information\. 

**Topics**
+ [Large model inference container image with DJL Serving](#realtime-endpoints-large-model-tutorials-deepspeed-djl-step1)
+ [Preparing your model artifacts](#realtime-endpoints-large-model-tutorials-deepspeed-djl-step2)
+ [Deploy the model using the SageMaker SDK](#realtime-endpoints-large-model-tutorials-deepspeed-djl-step3)

## Large model inference container image with DJL Serving<a name="realtime-endpoints-large-model-tutorials-deepspeed-djl-step1"></a>

 Large model inference \(LMI\) DLCs are available as Docker images on the Amazon Elastic Container Registry \(Amazon ECR\)\. These containers include the necessary components, libraries, and drivers to host large models on SageMaker or using Amazon EC2 infrastructure\. You can find a list of LMI DLCs in the [Large model inference](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#large-model-inference-containers) section of the DLCs list\. We use the following container in this tutorial: 


| Framework | Job type | CPU/GPU | Python version options | Example URL | 
| --- | --- | --- | --- | --- | 
| DJL Serving 0\.20\.0 with DeepSpeed 0\.7\.5, Hugging Face Transformers 4\.23\.1, and Hugging Face Accelerate 0\.13\.2 | inference | GPU | 3\.8 \(py38\) | 763104351884\.dkr\.ecr\.us\-west\-2\.amazonaws\.com/djl\-inference:0\.20\.0\-deepspeed0\.7\.5\-cu116 | 

 LMI DLCs are regularly updated and tested on SageMaker\. You can directly reference these Docker image URLs when you host on SageMaker, selecting the container that suits your need the most\. Each LMI DLC image includes DeepSpeed, Accelerate, Transformers and DJL Serving\. 

## Preparing your model artifacts<a name="realtime-endpoints-large-model-tutorials-deepspeed-djl-step2"></a>

 LMI DLCs use DJL Serving to serve your model for inference\. You have to configure DJL Serving and package your model in a format supported by DJL Serving and DeepSpeed for LMI\. 

1.  Create a `serving.properties` configuration file to indicate to DJL Serving which model parallelization and inference optimization libraries you would like to use\. You can find the configuration options for both DeepSpeed and Hugging Face Accelerate in [Configurations and settings](https://docs.aws.amazon.com/sagemaker/dg/realtime-endpoints-large-model-configuration)\. Modify the following example `serving.properties` file below to suit your needs\. 

   ```
   engine=DeepSpeed
   #engine=Python # for Hugging Face Accelerate
   option.entryPoint=djl_python.deepspeed
   #option.entryPoint=djl_python.huggingface # for Hugging Face Accelerate
   option.tensor_parallel_degree=2
   option.model_id=EleutherAI/gpt-j-6B
   #opiton.s3url=your-s3-url # downloads through s5cmd command, faster download
   option.task=text-generation
   option.dtype=fp16
   ```

    For a complete list of configuration options, see [Configurations and settings](realtime-endpoints-large-model-configuration.md)\. The following list describes some of the options used in the aforementioned example\. 
   +  **`option.entryPoint`** – This option is used to specify which handler offered by DJL Serving you would like to use\. The possible values are `huggingface`, `deepspeed`, and `stable-diffusion`\. Here we use `deepspeed`\. 
   +  **`option.model_id`** – Only provide this option if you also provide `option.entryPoint`\. The value of this option will be the Hugging Face ID of a model\. DJL Serving will use the ID to download the model from Hugging Face\. 
   +  **`option.s3url`** – Instead of specifying `option.model_id` and downloading from Hugging Face, you can alternatively download a model from an Amazon S3 bucket\. This option specifies the corresponding Amazon S3 URL\. DJL Serving uses `s5cmd` to download the model from the bucket\. 
   +  **`option.task`** – Model architectures generally require you specify a task as one of the parameters in `transformers.pipeline`\. 

    For more information, see the [DeepSpeed](https://github.com/deepjavalibrary/djl-serving/blob/master/engines/python/setup/djl_python/deepspeed.py), and [Hugging Face](https://github.com/deepjavalibrary/djl-serving/blob/master/engines/python/setup/djl_python/huggingface.py) handler implementations\. 

1.  Package your model artifacts into a compressed \.tar file\. DJL Serving and DeepSpeed expect the model artifacts to be packaged and formatted in a specific way\. This example uses the following directory structure for this purpose\. 

   ```
    - deepspeed-gptj # root directory
       - serving.properties            
       - model.py # your custom handler file, if you choose not to use the handlers provided with DJL Serving
       - model binary files # if you do not want to use option.model_id and option.s3_url
   ```

    The root directory contains the `serving.properties` file, along with any other necessary files\. This tutorial only needs the `serving.properties` file because it uses DJL Serving's handler and the `model_id` option to download the model directly from Hugging Face\. The following code snippet packages the model artifacts into a compressed \.tar file\. 

   ```
   mkdir deepspeed-gptj
   cp serving.properties deepspeed-gptj/                
   tar -czvf deepspeed-gptj.tar.gz deepspeed-gptj
   aws s3 sync deepspeed-gptj.targ.gz s3://djl-sm-test/tutorial/
   ```

## Deploy the model using the SageMaker SDK<a name="realtime-endpoints-large-model-tutorials-deepspeed-djl-step3"></a>

 This example uses SageMaker to handle the end to end model deployment process to an inference endpoint\. 

1. Create a SageMaker session\.

   ```
   import boto3
   import sagemaker
   
   aws_region = "aws-region"
   sagemaker_session = sagemaker.Session(boto_session=boto3.Session(region_name=aws_region))
   ```

1. Create a model in SageMaker\.

   ```
   from sagemaker.model import Model
   from sagemaker import image_uris, get_execution_role
   
   role = get_execution_role()
   
   def create_model(model_name, model_s3_url):
       # Get the DJL DeepSpeed image uri
       image_uri = image_uris.retrieve(
           framework="djl-deepspeed",
           region=sagemaker_session.boto_session.region_name,
           version="0.20.0"
       )
       model = Model(
           image_uri=image_uri,
           model_data=model_s3_url,
           role=role,
           name=model_name,
           sagemaker_session=sagemaker_session,
       )
       return model
   ```

    The `g5.12xlarge` instance has 4 Nvidia A10G GPUs\. The `serving.properties` in [Preparing your model artifacts](#realtime-endpoints-large-model-tutorials-deepspeed-djl-step2) specified a tensor parallel degree of 2\. DJL Serving automatically loads 2 copies of the model with 2 tensor parallel partitions to make use of all the 4 GPUs on the instance\. This doubles the throughput\. 

1. Deploy the model to a `g5.12xlarge` instance\.

   ```
   from sagemaker import serializers, deserializers
   
   def deploy_model(model, _endpoint_name):
       model.deploy(
           initial_instance_count=1,
           instance_type="ml.g5.12xlarge",
           endpoint_name=_endpoint_name
       )
       predictor = sagemaker.Predictor(
           endpoint_name=_endpoint_name,
           sagemaker_session=sagemaker_session,
           serializer=serializers.JSONSerializer(),
           deserializer=deserializers.JSONDeserializer()
       )
       return predictor
   ```

1. Make a prediction

   ```
   import argparse
   if __name__ == "__main__":
       arg_parser = argparse.ArgumentParser()
   
       group = arg_parser.add_mutually_group()
       group.add_argument("--deepspeed", action="store_const", dest="framework", const="deepspeed")
       group.add_argument("--accelerate", action="store_const", dest="framework", const="accelerate")
   
       args = arg_parser.parse_args()
       
       _model_name = f"{args.framework}-gptj"
       _model_s3_url = f"s3://djl-sm-test/tutorial/{args.framework}-gptj.tar.gz"
       _endpoint_name=f"{args.framework}-gptj"
   
       model = create_model(_model_name, _model_s3_url)
       predictor = deploy_model(model, _endpoint_name)
       print(predictor.predict(
                   { 
                       "inputs" : "Large model inference is", 
                       "parameters": { "max_length": 50 },
                   }
       ))
   ```