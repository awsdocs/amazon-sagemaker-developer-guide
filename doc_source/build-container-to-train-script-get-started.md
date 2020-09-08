# Get Started: Build Your Custom Training Container with Amazon SageMaker<a name="build-container-to-train-script-get-started"></a>

To run your own training model, build a Docker container using the [Amazon SageMaker Training Toolkit](https://github.com/aws/sagemaker-training-toolkit) through an Amazon SageMaker notebook instance\.

## Step 1: Create a SageMaker notebook instance<a name="byoc-training-step1"></a>

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Notebook**, choose **Notebook instances**, and then choose **Create notebook instance**\. 

1. On the **Create notebook instance** page, provide the following information: 

   1. For **Notebook instance name**, enter **RunScriptNotebookInstance**\.

   1. For **Notebook Instance type**, choose **ml\.t2\.medium**\.

   1. In the **Permissions and encryption** section, do the following:

      1. For **IAM role**, choose **Create a new role**\.

      1. On the **Create an IAM role** page, choose **Specific S3 buckets**, specify an S3 bucket named **sagemaker\-run\-script**, and then choose **Create role**\.

         SageMaker creates an IAM role named `AmazonSageMaker-ExecutionRole-YYYYMMDDTHHmmSS`\. For example, `AmazonSageMaker-ExecutionRole-20190429T110788`\. Note that the execution role naming convention uses the date and time when the role was created, separated by a `T`\.

   1. For **Root Access**, choose **Enable**\.

   1. Choose **Create notebook instance**\. 

1.  On the **Notebook instances** page, the **Status** is **Pending**\. It can take a few minutes for Amazon SageMaker to launch a machine learning compute instance—in this case, it launches a notebook instance—and attaches an ML storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server and a set of Anaconda libraries\. For more information, see [  CreateNotebookInstance](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html)\. 

1.  In the **Permissions and encryption** section, copy **the IAM role ARN number**, and paste into a notepad file to save it temporarily\. You will use this IAM role ARN number later to configure a local training estimator in the notebook instance\. **The IAM role ARN number** looks like the following: `'arn:aws:iam::111122223333:role/service-role/AmazonSageMaker-ExecutionRole-20190429T110788'` 

1. After the status of the notebook instance changes to **InService**, choose **Open JupyterLab**\.

## Step 2: Create and upload the Dockerfile and Python training scripts<a name="byoc-training-step2"></a>

1.  After JupyterLab opens, create a new folder in the home directory of your JupyterLab\. In the upper\-left corner, choose the **New Folder** icon, and then enter the folder name `docker_test_folder`\. 

1.  Create a `Dockerfile` text file in the `docker_test_folder` directory\. 

   1. Choose the **New Launcher** icon \(\+\) in the upper\-left corner\. 

   1. In the right pane under the **Other** section, choose **Text File**\.

   1.  Paste the following `Dockerfile` sample code into your text file\. 

      ```
      FROM tensorflow/tensorflow:2.2.0rc2-gpu-py3-jupyter
      
      # Install sagemaker-training toolkit to enable SageMaker Python SDK
      RUN pip3 install sagemaker-training
      
      # Copies the training code inside the container
      COPY train.py /opt/ml/code/train.py
      
      # Defines train.py as script entrypoint
      ENV SAGEMAKER_PROGRAM train.py
      ```

      The Dockerfile script performs the following tasks:
      + `FROM tensorflow/tensorflow:2.2.0rc2-gpu-py3-jupyter` – Downloads the TensorFlow docker base image\. You can replace this with any docker base image you want to bring to build containers as well as AWS pre\-built container base images\.
      + `RUN pip install sagemaker-training` – Installs [SageMaker Training Toolkit](https://github.com/aws/sagemaker-training-toolkit) that contains the common functionality necessary to create a container compatible with SageMaker\. 
      + `COPY train.py /opt/ml/code/train.py` – Copies the script to the location inside the container that is expected by SageMaker\. The script must be located in this folder\.
      + `ENV SAGEMAKER_PROGRAM train.py` – Takes your training script `train.py` as the entrypoint script copied in the `/opt/ml/code` folder of the container\. This is the only environmental variable that you must specify when you build your own container\.

   1.  On the left directory navigation pane, the text file name might automatically be named `untitled.txt`\. To rename the file, right\-click the file, choose **Rename**, rename the file as `Dockerfile` without the `.txt` extension, and then press `Ctrl+s` or `Command+s` to save the file\.

1.  Create or upload a training script `train.py` in the `docker_test_folder`\. You can use the following example script for this exercise\. 

   ```
   import tensorflow as tf
   
   mnist = tf.keras.datasets.mnist
   
   (x_train, y_train), (x_test, y_test) = mnist.load_data()
   x_train, x_test = x_train / 255.0, x_test / 255.0
   
   model = tf.keras.models.Sequential([
   tf.keras.layers.Flatten(input_shape=(28, 28)),
   tf.keras.layers.Dense(128, activation='relu'),
   tf.keras.layers.Dropout(0.2),
   tf.keras.layers.Dense(10, activation='softmax')
   ])
   
   model.compile(optimizer='adam',
   loss='sparse_categorical_crossentropy',
   metrics=['accuracy'])
   
   model.fit(x_train, y_train, epochs=1)
   
   model.evaluate(x_test, y_test)
   ```

## Step 3: Build the container<a name="byoc-training-step3"></a>

1. In the JupyterLab home directory, open a Jupyter notebook\. To open a new notebook, choose the **New Launch** icon and then choose **conda\_tensorflow2\_p36** in the **Notebook** section\. 

1. Run the following command in the first notebook cell to change to the `docker_test_folder` directory:

   ```
   % cd ~/SageMaker/docker_test_folder
   ```

   This returns your current directory as follows:

   ```
   ! pwd
   ```

   `output: /home/ec2-user/SageMaker/docker_test_folder`

1. To build the Docker container, run the following Docker build command, including the space followed by a period at the end:

   ```
   ! docker build -t tf-custom-container-test .
   ```

   The Docker build command must be run from the docker directory you created, in this case `docker_test_folder`\.
**Note**  
If you get the following error message that docker cannot find the Dockerfile, make sure the Dockerfile has the correct name and has been saved to the directory\.  

   ```
   unable to prepare context: unable to evaluate symlinks in Dockerfile path: 
   lstat /home/ec2-user/SageMaker/docker/Dockerfile: no such file or directory
   ```
Remember that `docker` looks for a file specifically called `Dockerfile` without any extension within the current directory\. If you named it something else, you can pass in the file name manually with the `-f` flag\. For example, if you named your Dockerfile as `Dockerfile-text.txt`, run the following command:  

   ```
   ! docker build -t tf-custom-container-test -f Dockerfile-text.txt .
   ```

## Step 4: Test the container<a name="byoc-training-step4"></a>

1. To test the container locally in the notebook instance, open a Jupyter notebook\. Choose **New Launcher** and choose **Notebook** in **`conda_tensorflow_p36`** framework\. 

1. Paste the following example script into the notebook code cell to configure a SageMaker Estimator\.

------
#### [ SageMaker Python SDK v1 ]

   ```
   from sagemaker.estimator import Estimator
   
   estimator = Estimator(image_name='tf-custom-container-test',
                         role='arn:aws:iam::111122223333:role/role-name',
                         train_instance_count=1,
                         train_instance_type='local')
   
   estimator.fit()
   ```

------
#### [ SageMaker Python SDK v2 ]

   ```
   from sagemaker.estimator import Estimator
   
   estimator = Estimator(image_uri='tf-custom-container-test',
                         role='arn:aws:iam::111122223333:role/role-name',
                         instance_count=1,
                         instance_type='local')
   
   estimator.fit()
   ```

------

1. Replace the `'Put_Your_ARN_Here'` value with **the IAM role ARN number** you copied to a notepad file when you configured the notebook instance\. The ARN should look like the following: `'arn:aws:iam::111122223333:role/service-role/AmazonSageMaker-ExecutionRole-20190429T110788'`\.

1. Run the code cell\. This test outputs the training environment configuration, the values used for the environmental variables, the source of the data, and the loss and accuracy obtained during training\.

## Step 5: Push the container to Amazon Elastic Container Registry \(Amazon ECR\)<a name="byoc-training-step5"></a>

1. After you successfully run the local mode test, you can push the docker container to [Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) and use it to run training jobs\. 

   Run the following command lines in a notebook cell\.

   ```
   %%sh
   
   # Specify an algorithm name
   algorithm_name=tf-custom-container-test
   
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
   
   docker build -t ${algorithm_name} .
   docker tag ${algorithm_name} ${fullname}
   
   docker push ${fullname}
   ```
**Note**  
This bash shell script may raise a permission issue similar to the following error message:  

   ```
   "denied: User: [ARN] is not authorized to perform: ecr:InitiateLayerUpload on resource: 
   arn:aws:ecr:us-east-1:[id]:repository/tf-custom-container-test"
   ```
If this error occurs, you need to attach the **AmazonEC2ContainerRegistryFullAccess** policy to your IAM role\. Go to the [IAM console](https://console.aws.amazon.com/iam/home), choose **Roles** from the left navigation pane, look up the IAM role you used for the Notebook instance\. Under the **Permission** tab, choose the **Attach policies** button, and search the **AmazonEC2ContainerRegistryFullAccess** policy\. Mark the check box of the policy, and choose **Attach policy** to finish\.

1. After you push the container, you can call the Amazon ECR image from anywhere in the SageMaker environment\. Run the following code example in the next notebook cell\. 

   If you want to use this training container with Studio to use its visualization features, you can also run the following code in a Studio notebook cell to call the Amazon ECR image of your training container\.

   ```
   import boto3
   
   account_id = boto3.client('sts').get_caller_identity().get('Account')
   ecr_repository = 'sagemaker-byoc-test'
   tag = ':latest'
   
   region = boto3.session.Session().region_name
   
   uri_suffix = 'amazonaws.com'
   if region in ['cn-north-1', 'cn-northwest-1']:
       uri_suffix = 'amazonaws.com.cn'
   
   byoc_image_uri = '{}.dkr.ecr.{}.{}/{}'.format(account_id, region, uri_suffix, ecr_repository + tag)
   
   byoc_image_uri
   # This should return something like
   # 111122223333.dkr.ecr.us-east-2.amazonaws.com/sagemaker-byoc-test:latest
   ```

1. Use the `ecr_image` retrieved from the previous step to configure a SageMaker estimator object\. The following code sample configures a SageMaker estimator with the `byoc_image_uri` and initiates a training job on an Amazon EC2 instance\.

------
#### [ SageMaker Python SDK v1 ]

   ```
   import sagemaker
   from sagemaker import get_execution_role
   from sagemaker.estimator import Estimator
   
   estimator = Estimator(image_name=byoc_image_uri,
                         role=get_execution_role(),
                         base_job_name='tf-custom-container-test-job',
                         train_instance_count=1,
                         train_instance_type='ml.p2.xlarge')
   
   # start training
   estimator.fit()
   
   # deploy the trained model
   predictor = estimator.deploy(1, instance_type)
   ```

------
#### [ SageMaker Python SDK v2 ]

   ```
   import sagemaker
   from sagemaker import get_execution_role
   from sagemaker.estimator import Estimator
   
   estimator = Estimator(image_uri=byoc_image_uri,
                         role=get_execution_role(),
                         base_job_name='tf-custom-container-test-job',
                         instance_count=1,
                         instance_type='ml.p2.xlarge')
   
   # start training
   estimator.fit()
   
   # deploy the trained model
   predictor = estimator.deploy(1, instance_type)
   ```

------

For a full example that shows how to test a custom container locally and push it to an Amazon ECR image, see the [ Building Your Own TensorFlow Container](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/tensorflow_bring_your_own/tensorflow_bring_your_own.ipynb) example notebook\.

## Step 6: Clean up resources<a name="byoc-training-step6"></a>

**To clean up resources when done with the get started example**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/), choose the notebook instance **RunScriptNotebookInstance**, choose **Actions**, and choose **Stop**\. It can take a few minutes for the instance to stop\. 

1. After the instance **Status** changes to **Stopped**, choose **Actions**, choose **Delete**, and then choose **Delete** in the dialog box\. It can take a few minutes for the instance to be deleted\. The notebook instance disappears from the table when it has been deleted\. 

1. Open the [Amazon S3 console](https://console.aws.amazon.com/s3/) and delete the bucket that you created for storing model artifacts and the training dataset\. 

1. Open the [IAM console](https://console.aws.amazon.com/iam/) and delete the IAM role\. If you created permission policies, you can delete them, too\. 
**Note**  
 The Docker container shuts down automatically after it has run\. You don't need to delete it\.