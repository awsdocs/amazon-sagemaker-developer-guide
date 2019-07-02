# Get Started: Use Amazon SageMaker Containers to Run a Python Script<a name="build-container-to-train-script-get-started"></a>

To run an arbitrary script\-based program in a Docker container using the Amazon SageMaker Containers, build a Docker container with an Amazon SageMaker notebook instance, as follows: 

1. Create the notebook instance\. 

1. Create and upload the Dockerfile and Python scripts\.

1. Build the container\.

1. Test the container locally\.

1. Clean up the resources\.

**To create an Amazon SageMaker notebook instance**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Notebook** , **Notebook instances**, and **Create notebook instance**\.

1. On the **Create notebook instance** page, provide the following information: 

   1. For **Notebook instance name**, enter **RunScriptNotebookInstance**\.

   1. For **Notebook Instance type**, choose **ml\.t2\.medium**\.

   1. For **IAM role**, choose **Create a new role**\.

      1. Choose **Create a new role**\.

      1. On the **Create an IAM role** page, choose **Specific S3 buckets**, specify an S3 bucket named **sagemaker\-run\-script**,\. and then choose **Create role**\.

         Amazon SageMaker creates an IAM role named `AmazonSageMaker-ExecutionRole-YYYYMMDDTHHmmSS`\. For example, `AmazonSageMaker-ExecutionRole-20190429T110788`\. Record the role name because you'll need it later\.

   1. For **Root Access**, accept the default, **Enabled**\.

   1. Choose **Create notebook instance**\. 

      It takes a few minutes for Amazon SageMaker to launch an ML compute instance—in this case, a notebook instance—and attach an ML storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server and a set of Anaconda libraries\. For more information, see the [CreateNotebookInstance](API_CreateNotebookInstance.md) API\. 

1. When the status of the notebook instance is `InService`, from **Actions**, choose **Open Jupyter**\.

   For **New**, choose **conda\_tensorflw\_p36**\. This is the kernel you need\.

1. To name the notebook, choose **File**, **Rename**, enter t**Run\-Python\-Script**, and then choose **Rename**\.

**To create and upload the Dockerfile and Python scripts**

1. In the editor of your choice, create the following Dockerfile text file locally and save it with the file name "Dockerfile" without an extension\. The `docker build` command expects by default to find a file with precisely this name in the dockerfile directory\. For example, in Notepad, you can save a text file without an extension by choosing **File**, **Save As** and choosing **All types\(\*\.\*\)**\.

   ```
   FROM tensorflow/tensorflow:2.0.0a0
   
   RUN pip install sagemaker-containers
   
   # Copies the training code inside the container
   COPY train.py /opt/ml/code/train.py
   
   # Defines train.py as script entrypoint
   ENV SAGEMAKER_PROGRAM train.py
   ```

   The Dockerfile script performs the following tasks:
   + `FROM tensorflow/tensorflow:2.0.0a0` downloads the TensorFlow library used to run the Python script\.
   + `RUN pip install sagemaker-containers`[Amazon SageMaker Containers](https://github.com/aws/sagemaker-containers) contains the common functionality necessary to create a container compatible with Amazon SageMaker\. 
   + `COPY train.py /opt/ml/code/train.py` copies the script to the location inside the container that is expected by Amazon SageMaker\. The script must be located in this folder\.
   + `ENV SAGEMAKER_PROGRAM train.py` defines train\.py as the name of the entrypoint script that is located in the /opt/ml/code folder for the container\. This is the only environmental variable that you must specify when, you are using your own container\.

1. In the editor of your choice, create and save the following `train.py` text file locally\.

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

1. To upload the Dockerfile to a dockerfile directory, choose **Open JupyterLab**, choose the **File Browser** icon, and then choose the **New Folder** icon\. This creates a new directory named **dockerfile**\.

1. Double\-click the new `dockerfile` folder, choose the **Upload Files** icon, navigate to where you saved your Dockerfile and `train.py` script files, and upload them to the `dockerfile` folder\.

**To build the container**

1. The Jupyter Notebook opens in the SageMaker directory\. The Docker build command must be run from the` dockerfile` directory you created\. Run the following command to change into the `dockerfile` directory:

   ```
   cd dockerfile
   ```

   This returns your current directory: `/home/ec2-user/SageMaker/dockerfile`

1. To build the Docker container, run the following Docker build command, including the final period\.

   ```
   !docker build -t tf-2.0 .
   ```

**To test the container locally**

1. Use Local Mode the test the container locally\. Replace the `'SageMakerRole'` value with the ARN for the role with the IAM role you created when configuring the notebook instance\. The ARN should look like:` 'arn:aws:iam::109225375568:role/service-role/AmazonSageMaker-ExecutionRole-20190429T110788'`\.

   ```
   from sagemaker.estimator import Estimator 
   
   estimator = Estimator(image_name='tf-2.0',
   			  role='SageMakerRole',
   			  train_instance_count=1,
   			  train_instance_type='local')
   	
   estimator.fit()
   ```

   This test outputs the training environment configuration, the values used for the environmental variables, the source of the data, and the loss and accuracy obtained during training\.

1. After using Local Mode, you can push the image to Amazon Elastic Container Registry and use it to run training jobs\. For an example that shows how to complete these tasks, see [Building Your Own TensorFlow Container](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/tensorflow_bring_your_own/tensorflow_bring_your_own.ipynb)

**To clean up resources when done with the get started example**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/), stop and then delete the notebook instance\. 

1. Open the Amazon S3console at [https://console\.aws\.amazon\.com/s3](https://console.aws.amazon.com/s3/) and delete the bucket that you created for storing model artifacts and the training dataset\. 

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) and delete the IAM role\. If you created permission policies, you can delete them, too\. 
**Note**  
 The Docker container shuts down automatically after it has run\. You don't need to delete it\.