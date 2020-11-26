# Build Your Own Container with Multi Model Server<a name="build-multi-model-build-container"></a>

Custom Elastic Container Registry \(ECR\) images deployed in Amazon SageMaker are expected to adhere to the basic contract described in [Use Your Own Inference Code with Hosting Services](your-algorithms-inference-code.md) that govern how SageMaker interacts with a Docker container that runs your own inference code\. For a container to be capable of loading and serving multiple models concurrently, there are additional APIs and behaviors that must be followed\. This additional contract includes new APIs to load, list, get, and unload models, and a different API to invoke models\. There are also different behaviors for error scenarios that the APIs need to abide by\. To indicate that the container complies with the additional requirements, you can add the following command to your Docker file:

```
LABEL com.amazonaws.sagemaker.capabilities.multi-models=true
```

SageMaker also injects an environment variable into the container

```
SAGEMAKER_MULTI_MODEL=true
```

If you are creating a multi\-model endpoint for a serial inference pipline, your Docker file must have the required labels for both multi\-models and serial inference pipelines\. For more information about serial information pipelines, see [Run Real\-time Predictions with an Inference Pipeline](inference-pipeline-real-time.md)\.

To help you implement these requirements for a custom container, two libraries are available:
+ [Multi Model Server](https://github.com/awslabs/multi-model-server) is an open source framework for serving machine learning models that can be installed in containers to provide the front end that fulfills the requirements for the new multi\-model endpoint container APIs\. It provides the HTTP front end and model management capabilities required by multi\-model endpoints to host multiple models within a single container, load models into and unload models out of the container dynamically, and performs inference on a specified loaded model\. It also provides a pluggable backend that supports a pluggable custom backend handler where you can implement your own algorithm\.
+ [SageMaker Inference Toolkit](https://github.com/aws/sagemaker-inference-toolkit) is a library that bootstraps Multi Model Server with a configuration and settings that make it compatible with SageMaker multi\-model endpoints\. It also allows you to tweak important performance parameters, such as the number of workers per model, depending on the needs of your scenario\. 

## Use the SageMaker Inference Toolkit<a name="multi-model-inference-toolkit"></a>

Currently, the only pre\-built containers that support multi\-model endpoints are the MXNet inference container and the PyTorch inference container\. If you want to use any other framework or algorithm, you need to build a container\. The easiest way to do this is to use the [SageMaker Inference Toolkit](https://github.com/aws/sagemaker-inference-toolkit) to extend an existing pre\-built container\. The SageMaker inference toolkit is an implementation for the multi\-model server \(MMS\) that creates endpoints that can be deployed in SageMaker\. For a sample notebook that shows how to set up and deploy a custom container that supports multi\-model endpoints in SageMaker, see the [Multi\-Model Endpoint BYOC Sample Notebook](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/multi_model_bring_your_own)\.

**Note**  
The SageMaker inference toolkit supports only Python model handlers\. If you want to implement your handler in any other language, you must build your own container that implements the additional multi\-model endpoint APIs\. For information, see [Contract for Custom Containers to Serve Multiple Model](mms-container-apis.md)\.

**To extend a container by using the SageMaker inference toolkit**

1. Create a model handler\. MMS expects a model handler, which is a Python file that implements functions to pre\-process, get preditions from the model, and process the output in a model handler\. For an example of a model handler, see [model\_handler\.py](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/multi_model_bring_your_own/container/model_handler.py) from the sample notebook\.

1. Import the inference toolkit and use its `model_server.start_model_server` function to start MMS\. The following example is from the `dockerd-entrypoint.py` file from the sample notebook\. Notice that the call to `model_server.start_model_server` passes the model handler described in the previous step:

   ```
   import subprocess
   import sys
   import shlex
   import os
   from retrying import retry
   from subprocess import CalledProcessError
   from sagemaker_inference import model_server
   
   def _retry_if_error(exception):
       return isinstance(exception, CalledProcessError or OSError)
   
   @retry(stop_max_delay=1000 * 50,
          retry_on_exception=_retry_if_error)
   def _start_mms():
       # by default the number of workers per model is 1, but we can configure it through the
       # environment variable below if desired.
       # os.environ['SAGEMAKER_MODEL_SERVER_WORKERS'] = '2'
       model_server.start_model_server(handler_service='/home/model-server/model_handler.py:handle')
   
   def main():
       if sys.argv[1] == 'serve':
           _start_mms()
       else:
           subprocess.check_call(shlex.split(' '.join(sys.argv[1:])))
   
       # prevent docker exit
       subprocess.call(['tail', '-f', '/dev/null'])
       
   main()
   ```

1. In your `Dockerfile`, copy the model handler from the first step and specify the Python file from the previous step as the entrypoint in your `Dockerfile`\. The following lines are from the [Dockerfile](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/multi_model_bring_your_own/container/Dockerfile) used in the sample notebook:

   ```
   # Copy the default custom service file to handle incoming data and inference requests
   COPY model_handler.py /home/model-server/model_handler.py
   
   # Define an entrypoint script for the docker image
   ENTRYPOINT ["python", "/usr/local/bin/dockerd-entrypoint.py"]
   ```

1. Build and register your container\. The following shell script from the sample notebook builds the container and uploads it to an Amazon Elastic Container Registry repository in your AWS account:

   ```
   %%sh
   
   # The name of our algorithm
   algorithm_name=demo-sagemaker-multimodel
   
   cd container
   
   account=$(aws sts get-caller-identity --query Account --output text)
   
   # Get the region defined in the current configuration (default to us-west-2 if none defined)
   region=$(aws configure get region)
   region=${region:-us-west-2}
   
   fullname="${account}.dkr.ecr.${region}.amazonaws.com/${algorithm_name}:latest"
   
   # If the repository doesn't exist in ECR, create it.
   aws ecr describe-repositories --repository-names "${algorithm_name}" > /dev/null 2>&1
   
   if [ $? -ne 0 ]
   then
       aws ecr create-repository --repository-name "${algorithm_name}" > /dev/null
   fi
   
   # Get the login command from ECR and execute it directly
   $(aws ecr get-login --region ${region} --no-include-email)
   
   # Build the docker image locally with the image name and then push it to ECR
   # with the full name.
   
   docker build -q -t ${algorithm_name} .
   docker tag ${algorithm_name} ${fullname}
   
   docker push ${fullname}
   ```

You can now use this container to deploy multi\-model endpoints in SageMaker\.

**Topics**
+ [Use the SageMaker Inference Toolkit](#multi-model-inference-toolkit)
+ [Contract for Custom Containers to Serve Multiple Model](mms-container-apis.md)