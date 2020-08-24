# Use Debugger in Custom Training Containers<a name="debugger-bring-your-own-container"></a>

Amazon SageMaker Debugger is available for any deep learning models that you bring to Amazon SageMaker\. The AWS CLI, Amazon SageMaker `Estimator` API, and the Debugger APIs enable you to use any Docker base images to build and customize containers to train your models\. To use Debugger with the customized containers, you need to make a minimal change to your training script to implement the Debugger hook callback and retrieve tensors from training jobs\.

You need the following resources to build a customized container with Debugger\.
+ [Amazon SageMaker Estimator API](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html)
+ [The open source smdebug client library](https://github.com/awslabs/sagemaker-debugger)
+ A Docker base image of your choice
+ Your training script with a minimal modification to implement the Debugger hook \- You can find instructions and examples of modifying your training script from [Debugger in AWS Containers with Script Mode](debugger-container.md#debugger-script-mode)\.

## Prepare to Build a Custom Training Container<a name="debugger-bring-your-own-container-1"></a>

To build a docker container, the base structure of files should look like the following:

```
├── debugger_custom_container_test_notebook.ipynb      # a notebook to run python snippet codes
└── debugger_custom_container_test_folder              # this is a docker folder
    ├──  your-training-script.py                       # your training script modified to implement Debugger hook
    └──  Dockerfile                                    # a Dockerfile to build your own container
```

## Create and Configure a Dockerfile<a name="debugger-bring-your-own-container-2"></a>

Open your SageMaker JupyterLab and create a new folder, `debugger_custom_container_test_folder` in this example, to save your training script and `Dockerfile`\. The following code example is a `Dockerfile` that includes essential docker build commends\. Paste the code into the `Dockerfile` text file and save it\. Upload your training script to the same folder\.

```
# Specify a docker base image
FROM tensorflow/tensorflow:2.2.0rc2-gpu-py3-jupyter

# Install required packages to enable the SageMaker Python SDK and the smdebug library
RUN pip install sagemaker-training
RUN pip install smdebug

# Copies the training code inside the container
COPY your-training-script.py /opt/ml/code/your-training-script.py

# Defines your modified training script as the entry_point
ENV SAGEMAKER_PROGRAM your-training-script.py
```

If you want to use a pre\-built AWS Deep Learning Container image, see [Available AWS Deep Learning Containers Images](https://aws.amazon.com/releasenotes/available-deep-learning-containers-images/)\.

## Build and Push the Custom Training Container to Amazon ECR<a name="debugger-bring-your-own-container-3"></a>

Create a test notebook, `debugger_custom_container_test_notebook.ipynb`, and run the following code in the notebook cell\. This will access the `debugger_byoc_test_docker` directory, build the docker with the specified `algorithm_name`, and push the docker container to your Amazon ECR\.

```
%%sh

cd debugger_custom_container_test_folder

# Specify the name of your algorithm
algorithm_name=your-algorithm

account=$(aws sts get-caller-identity --query Account --output text)

# Get the region defined in the current configuration
region=$(aws configure get region)

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

docker build  -t ${algorithm_name} .
docker tag ${algorithm_name} ${fullname}

docker push ${fullname}
```

For a full documentation about pushing a docker container to an Amazon ECR, see [Pushing a Docker image to an Amazon ECR repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)\.

For an end\-to\-end example of pushing a customized container to an Amazon ECR, see [Building your own TensorFlow container](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/tensorflow_bring_your_own/tensorflow_bring_your_own.ipynb)\.

## Run and Debug Training Jobs Using the Custom Training Container<a name="debugger-bring-your-own-container-4"></a>

After you build and push your docker container to an Amazon ECR, you can locate the `ecr_image` by using the AWS `boto3` Python SDK as shown in the following code example\.

```
import boto3

client = boto3.client('sts')
account = client.get_caller_identity()['Account']

my_session = boto3.session.Session()
region = my_session.region_name

algorithm_name = "your-algorithm"
ecr_image = '{}.dkr.ecr.{}.amazonaws.com/{}:latest'.format(account, region, algorithm_name)

ecr_image
# This should return something like
# 12-digits-of-your-account.dkr.ecr.us-east-2.amazonaws.com/your-algorithm:latest
```

Use the `ecr_image` to configure a SageMaker Estimator and enable the Debugger features\.

```
import sagemaker

from sagemaker import get_execution_role
from sagemaker.estimator import Estimator
from sagemaker.debugger import Rule, DebuggerHookConfig, CollectionConfig, rule_configs

estimator = Estimator(image_name=ecr_image,
    role=get_execution_role(),
    base_job_name='debugger-custom-container-test',
    train_instance_count=1,
    train_instance_type='ml.p2.xlarge',
    
    ## Debugger parameters
    rules=[ Rule.sagemaker(rules_config.vanishing_gradient()),
            ... ],
    debugger_hook_config=DebuggerHookConfig(
                            s3_output_path=s3_bucket_for_tensors,  # Required
                            collection_configs=[ CollectionConfig(name="gradients") ]
                         )
)

# start training
estimator.fit()

# deploy the trained model
predictor = estimator.deploy(1, instance_type)
```