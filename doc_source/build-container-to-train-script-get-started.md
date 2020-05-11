# Get Started: Use Amazon SageMaker Containers to Run a Python Script<a name="build-container-to-train-script-get-started"></a>

To run your own training model using the Amazon SageMaker Containers, build a Docker container through an Amazon SageMaker Notebook instance\. 

**To create an Amazon SageMaker notebook instance**

1. Open [the Amazon SageMaker Dashboard](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Notebook**, **Notebook instances**, and **Create notebook instance**\. 

1. On the **Create notebook instance** page, provide the following information: 

   1. For **Notebook instance name**, enter **RunScriptNotebookInstance**\.

   1. For **Notebook Instance type**, choose **ml\.t2\.medium**\.

   1. For **IAM role**, choose **Create a new role**\.

      1. Choose **Create a new role**\.

      1. On the **Create an IAM role** page, choose **Specific S3 buckets**, specify an S3 bucket named **sagemaker\-run\-script**, and then choose **Create role**\.

         Amazon SageMaker creates an IAM role named `AmazonSageMaker-ExecutionRole-YYYYMMDDTHHmmSS`\. For example, `AmazonSageMaker-ExecutionRole-20190429T110788`\. Note that the execution role naming convention uses the date and time when the role was created, separated by a `T`\. Record the role name because you'll need it later\.

   1. For **Root Access**, choose **Enabled**\.

   1. Choose **Create notebook instance**\. 

      It takes a few minutes for Amazon SageMaker to launch an ML compute instance—in this case, a notebook instance—and attach an ML storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server and a set of Anaconda libraries\. For more information, see the [  `CreateNotebookInstance`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html) API\. 

1.  Choose RunScriptNotebookInstance, scroll down to **Permissions and encryption** section, and record **the IAM role ARN number** in a notepad\. **The IAM role ARN number** looks like the following: `'arn:aws:iam::109225375568:role/service-role/AmazonSageMaker-ExecutionRole-20190429T110788'` 

1. When the status of the notebook instance is `InService`, from **Actions**, choose **Open JupyterLab**\.

**To create and upload the Dockerfile and Python training scripts**

1.  On the left top corner, choose New Folder icon\. Name the folder `docker_test_folder`\. 

1.  Create a `Dockerfile` text file\. 

   1.  In the `docker_test_folder` directory, choose New Launcher icon \(\+ mark\) on the top left corner\. In the right panel, select **Text File** under the **Other** section\. 

   1.  Copy and paste the following `Dockerfile` code into your newly created text file\. 

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
      + `RUN pip install sagemaker-containers` [Amazon SageMaker Containers](https://github.com/aws/sagemaker-containers) contains the common functionality necessary to create a container compatible with Amazon SageMaker\. 
      + `COPY train.py /opt/ml/code/train.py` copies the script to the location inside the container that is expected by Amazon SageMaker\. The script must be located in this folder\.
      + `ENV SAGEMAKER_PROGRAM train.py` defines train\.py as the name of the entrypoint script that is located in the /opt/ml/code folder for the container\. This is the only environmental variable that you must specify when, you are using your own container\.

   1.  Make sure you rename the text file name `Dockerfile`\. On the left directory navigation, the text file name is automatically set as `untitled.txt`\. Open a context manu \(pop\-up menu\) for the file, choose **Rename**, and rename the file as `Dockerfile` without `.txt` extension\. Make sure you **save the file** by hitting `Ctrl+s` or `Command+s`\.

1.  In the same way, create a `train.py` file with your own training script\. You can test using the following example `train.py` script\. 

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

**To build the container**

1. The Jupyter Notebook instance opens in the SageMaker directory\. The Docker build command must be run from the `docker` directory you created, in this case `docker_test_folder`\. To open a new terminal window on your notebook instance, choose **New Launch** icon, select **Terminal** under **Other** section, and run the following command to change into the `docker_test_folder` directory\.

   ```
   cd SageMaker/docker_test_folder
   ```

   This returns your current directory as follows\.

   ```
   pwd
   ```

   `output: /home/ec2-user/SageMaker/docker_test_folder`

1. To build the Docker container, run the following Docker build command including the final period\.

   ```
   docker build -t tf-2.0 .
   ```
**Note**  
If you get an error that it can't find the Dockerfile, make sure the files have correct names and are saved\. Remember that `docker` is looking for a file called `Dockerfile` in the current directory\. If you named it something else, you can pass in the filename manually with the `-f` argument\. For example:   

   ```
   docker build -t tf-2.0 -f Dockerfile.txt .
   ```

**To test the container**

1. To test the container locally to the Notebook instance, open a Jupyter notebook\. Choose **New Launcher** and select **Notebook** in **`conda_tensorflow_p36`** framework\. 

1. Copy and paste the following example script to the Notebook code cell to configure a Amazon SageMaker Estimator\.

   ```
   from sagemaker.estimator import Estimator
   
   estimator = Estimator(image_name='tf-2.0',
   	  role='Put_Your_ARN_Here',
   	  train_instance_count=1,
   	  train_instance_type='local')
   
   estimator.fit()
   ```

1. Replace the `'Put_Your_ARN_Here'` value with **the IAM role ARN number** you recorded while configuring the notebook instance\. The ARN should look like: `'arn:aws:iam::109225375568:role/service-role/AmazonSageMaker-ExecutionRole-20190429T110788'`\.

1. Run the code cell\. This test outputs the training environment configuration, the values used for the environmental variables, the source of the data, and the loss and accuracy obtained during training\.

After you successfully run this local mode test, you can push the image to Amazon Elastic Container Registry and use it to run training jobs\. For examples that show how to complete these tasks, see [Building Your Own TensorFlow Container](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/tensorflow_bring_your_own/tensorflow_bring_your_own.ipynb)

**To clean up resources when done with the get started example**

1. Open [the Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/), select the Notebook instance **RunScriptNotebookInstance**, choose **Actions**, and **Stop** the instance\. After the instance stops, **Delete** will be activated\. **Delete** the notebook instance\. 

1. Open [the Amazon S3console](https://console.aws.amazon.com/s3/) and delete the bucket that you created for storing model artifacts and the training dataset\. 

1. Open [the IAM console](https://console.aws.amazon.com/iam/) and delete the IAM role\. If you created permission policies, you can delete them, too\. 
**Note**  
 The Docker container shuts down automatically after it has run\. You don't need to delete it\.