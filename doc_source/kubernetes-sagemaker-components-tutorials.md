# Run Kubeflow Pipelines<a name="kubernetes-sagemaker-components-tutorials"></a>

In this tutorial, you run a pipeline using SageMaker Components for Kubeflow Pipelines to train a classification model using Kmeans with the MNIST dataset on SageMaker\. The workflow uses Kubeflow Pipelines as the orchestrator and SageMaker to execute each step of the workflow\. The example was taken from an existing [ SageMaker example](https://github.com/aws/amazon-sagemaker-examples/blob/8279abfcc78bad091608a4a7135e50a0bd0ec8bb/sagemaker-python-sdk/1P_kmeans_highlevel/kmeans_mnist.ipynb) and modified to work with SageMaker Components for Kubeflow Pipelines\.

You can define your pipeline in Python using AWS SDK for Python \(Boto3\) then use the KFP dashboard, KFP CLI, or Boto3 to compile, deploy, and run your workflows\. The full code for the MNIST classification pipeline example is available in the [Kubeflow Github repository](https://github.com/kubeflow/pipelines/tree/master/samples/contrib/aws-samples/mnist-kmeans-sagemaker#mnist-classification-with-kmeans)\. To use it, clone the Python files to your gateway node\.

You can find additional [ SageMaker Kubeflow Pipelines examples](https://github.com/kubeflow/pipelines/tree/master/samples/contrib/aws-samples) on GitHub\. For information on the components used, see the [KubeFlow Pipelines GitHub repository](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker)\.

To run the classification pipeline example, create a SageMaker IAM execution role granting your training job the permission to access AWS resources, then continue with the steps that correspond to your deployment option\.

## Create a SageMaker execution role<a name="create-an-amazonsagemaker-execution-role"></a>

The `kfp-example-sagemaker-execution-role` IAM role is a runtime role assumed by SageMaker jobs to access AWS resources\. In the following command, you create an IAM execution role named `kfp-example-sagemaker-execution-role`, attach two managed policies \(AmazonSageMakerFullAccess, AmazonS3FullAccess\), and create a trust relationship with SageMaker to grant SageMaker jobs access to those AWS resources\.

You provide this role as an input parameter when running the pipeline\.

Run the following command to create the role\. Note the ARN that is returned in your output\.

```
SAGEMAKER_EXECUTION_ROLE_NAME=kfp-example-sagemaker-execution-role

TRUST="{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Effect\": \"Allow\", \"Principal\": { \"Service\": \"sagemaker.amazonaws.com\" }, \"Action\": \"sts:AssumeRole\" } ] }"
aws iam create-role --role-name ${SAGEMAKER_EXECUTION_ROLE_NAME} --assume-role-policy-document "$TRUST"
aws iam attach-role-policy --role-name ${SAGEMAKER_EXECUTION_ROLE_NAME} --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerFullAccess
aws iam attach-role-policy --role-name ${SAGEMAKER_EXECUTION_ROLE_NAME} --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

aws iam get-role --role-name ${SAGEMAKER_EXECUTION_ROLE_NAME} --output text --query 'Role.Arn'
```

## Full Kubeflow on AWS Deployment<a name="run-pipelines-on-full-kubeflow-deployment"></a>

Follow the instructions of the [SageMaker Training Pipeline tutorial for MNIST Classification with K\-Means](https://awslabs.github.io/kubeflow-manifests/docs/amazon-sagemaker-integration/sagemaker-components-for-kubeflow-pipelines/)\.

## Standalone Kubeflow Pipelines Deployment<a name="run-pipelines-on-standalone-kubeflow-pipelines-deployment"></a>

### Prepare datasets<a name="prepare-datasets"></a>

To run the pipelines, you need to upload the data extraction pre\-processing script to an Amazon S3 bucket\. This bucket and all resources for this example must be located in the `us-east-1` region\. For information on creating a bucket, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

From the `mnist-kmeans-sagemaker` folder of the Kubeflow repository you cloned on your gateway node, run the following command to upload the `kmeans_preprocessing.py` file to your Amazon S3 bucket\. Change `<bucket-name>` to the name of your Amazon S3 bucket\.

```
aws s3 cp mnist-kmeans-sagemaker/kmeans_preprocessing.py s3://<bucket-name>/mnist_kmeans_example/processing_code/kmeans_preprocessing.py
```

### Compile and deploy your pipeline<a name="compile-and-deploy-your-pipeline"></a>

After defining the pipeline, you must compile it to an intermediate representation before you submit it to the Kubeflow Pipelines service on your cluster\. The intermediate representation is a workflow specification in the form of a YAML file compressed into a tar\.gz file\. You need the KFP SDK to compile your pipeline\.

#### Install KFP SDK<a name="install-kfp-sdk"></a>

Run the following from the command line of your gateway node:

1. Install the KFP SDK following the instructions in the [Kubeflow pipelines documentation](https://www.kubeflow.org/docs/pipelines/sdk/install-sdk/)\.

1. Verify that the KFP SDK is installed with the following command:

   ```
   pip show kfp
   ```

1. Verify that `dsl-compile` has been installed correctly as follows:

   ```
   which dsl-compile
   ```

#### Compile your pipeline<a name="compile-your-pipeline"></a>

You have three options to interact with Kubeflow Pipelines: KFP UI, KFP CLI, or the KFP SDK\. The following sections illustrate the workflow using the KFP UI and CLI\.

Complete the following steps from your gateway node\.

1. Modify your Python file with your Amazon S3 bucket name and IAM role ARN\.

1. Use the `dsl-compile` command from the command line to compile your pipeline as follows\. Replace `<path-to-python-file>` with the path to your pipeline and `<path-to-output>` with the location where you want your tar\.gz file to be\.

   ```
   dsl-compile --py <path-to-python-file> --output <path-to-output>
   ```

#### Upload and run the pipeline using the KFP CLI<a name="upload-and-run-the-pipeline-using-the-kfp-cli"></a>

Complete the following steps from the command line of your gateway node\. KFP organizes runs of your pipeline as experiments\. You have the option to specify an experiment name\. If you do not specify one, the run will be listed under **Default** experiment\.

1. Upload your pipeline as follows:

   ```
   kfp pipeline upload --pipeline-name <pipeline-name> <path-to-output-tar.gz>
   ```

   Your output should look like the following\. Take note of the pipeline `ID`\.

   ```
   Pipeline 29c3ff21-49f5-4dfe-94f6-618c0e2420fe has been submitted
   
   Pipeline Details
   ------------------
   ID           29c3ff21-49f5-4dfe-94f6-618c0e2420fe
   Name         sm-pipeline
   Description
   Uploaded at  2020-04-30T20:22:39+00:00
   ...
   ...
   ```

1. Create a run using the following command\. The KFP CLI run command currently does not support specifying input parameters while creating the run\. You need to update your parameters in the Python pipeline file before compiling\. Replace `<experiment-name>` and `<job-name>` with any names\. Replace `<pipeline-id>` with the ID of your submitted pipeline\. Replace `<your-role-arn>` with the ARN of `kfp-example-pod-role`\. Replace `<your-bucket-name>` with the name of the Amazon S3 bucket you created\. 

   ```
   kfp run submit --experiment-name <experiment-name> --run-name <job-name> --pipeline-id <pipeline-id> role_arn="<your-role-arn>" bucket_name="<your-bucket-name>"
   ```

   You can also directly submit a run using the compiled pipeline package created as the output of the `dsl-compile` command\.

   ```
   kfp run submit --experiment-name <experiment-name> --run-name <job-name> --package-file <path-to-output> role_arn="<your-role-arn>" bucket_name="<your-bucket-name>"
   ```

   Your output should look like the following:

   ```
   Creating experiment aws.
   Run 95084a2c-f18d-4b77-a9da-eba00bf01e63 is submitted
   +--------------------------------------+--------+----------+---------------------------+
   | run id                               | name   | status   | created at                |
   +======================================+========+==========+===========================+
   | 95084a2c-f18d-4b77-a9da-eba00bf01e63 | sm-job |          | 2020-04-30T20:36:41+00:00 |
   +--------------------------------------+--------+----------+---------------------------+
   ```

1. Navigate to the UI to check the progress of the job\.

#### Upload and run the pipeline using the KFP UI<a name="upload-and-run-the-pipeline-using-the-kfp-ui"></a>

1. On the left panel, choose the **Pipelines** tab\. 

1. In the upper\-right corner, choose **\+UploadPipeline**\. 

1. Enter the pipeline name and description\. 

1. Choose **Upload a file** and enter the path to the tar\.gz file you created using the CLI or with AWS SDK for Python \(Boto3\)\.

1. On the left panel, choose the **Pipelines** tab\.

1. Find the pipeline you created\.

1. Choose **\+CreateRun**\.

1. Enter your input parameters\.

1. Choose **Run**\.

### Run predictions<a name="running-predictions"></a>

Once your classification pipeline is deployed, you can run classification predictions against the endpoint that was created by the Deploy component\. Use the KFP UI to check the output artifacts for `sagemaker-deploy-model-endpoint_name`\. Download the \.tgz file to extract the endpoint name or check the SageMaker console in the region you used\.

#### Configure permissions to run predictions<a name="configure-permissions-to-run-predictions"></a>

If you want to run predictions from your gateway node, skip this section\.

**To use any other machine to run predictions, assign the `sagemaker:InvokeEndpoint` permission to the IAM role used by the client machine\.**

1. On your gateway node, run the following to create an IAM policy file:

   ```
   cat <<EoF > ./sagemaker-invoke.json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "sagemaker:InvokeEndpoint"
               ],
               "Resource": "*"
           }
       ]
   }
   EoF
   ```

1. Attach the policy to the IAM role of the client node\.

   Run the following command\. Replace `<your-instance-IAM-role>` with the name of the IAM role\. Replace `<path-to-sagemaker-invoke-json>` with the path to the policy file you created\.

   ```
   aws iam put-role-policy --role-name <your-instance-IAM-role> --policy-name sagemaker-invoke-for-worker --policy-document file://<path-to-sagemaker-invoke-json>
   ```

#### Run predictions<a name="run-predictions"></a>

1. Create a Python file from your client machine named `mnist-predictions.py` with the following content\. Replace the `ENDPOINT_NAME` variable\. The script loads the MNIST dataset, creates a CSV from those digits, then sends the CSV to the endpoint for prediction and prints the results\.

   ```
   import boto3
   import gzip
   import io
   import json
   import numpy
   import pickle
   
   ENDPOINT_NAME='<endpoint-name>'
   region = boto3.Session().region_name
   
   # S3 bucket where the original mnist data is downloaded and stored
   downloaded_data_bucket = f"jumpstart-cache-prod-{region}"
   downloaded_data_prefix = "1p-notebooks-datasets/mnist"
   
   # Download the dataset
   s3 = boto3.client("s3")
   s3.download_file(downloaded_data_bucket, f"{downloaded_data_prefix}/mnist.pkl.gz", "mnist.pkl.gz")
   
   # Load the dataset
   with gzip.open('mnist.pkl.gz', 'rb') as f:
       train_set, valid_set, test_set = pickle.load(f, encoding='latin1')
   
   # Simple function to create a csv from our numpy array
   def np2csv(arr):
       csv = io.BytesIO()
       numpy.savetxt(csv, arr, delimiter=',', fmt='%g')
       return csv.getvalue().decode().rstrip()
   
   runtime = boto3.Session(region).client('sagemaker-runtime')
   
   payload = np2csv(train_set[0][30:31])
   
   response = runtime.invoke_endpoint(EndpointName=ENDPOINT_NAME,
                                      ContentType='text/csv',
                                      Body=payload)
   result = json.loads(response['Body'].read().decode())
   print(result)
   ```

1. Run the Python file as follows: 

   ```
   python mnist-predictions.py
   ```

### View results and logs<a name="view-results-and-logs"></a>

When the pipeline is running, you can choose any component to check execution details, such as inputs and outputs\. This lists the names of created resources\.

If the KFP request is successfully processed and an SageMaker job is created, the component logs in the KFP UI provide a link to the job created in SageMaker\. The CloudWatch logs are also provided if the job is successfully created\. 

If you run too many pipeline jobs on the same cluster, you may see an error message that indicates that you do not have enough pods available\. To fix this, log in to your gateway node and delete the pods created by the pipelines you are not using:

```
kubectl get pods -n kubeflow
kubectl delete pods -n kubeflow <name-of-pipeline-pod>
```

### Cleanup<a name="cleanup"></a>

When you’re finished with your pipeline, you need to clean up your resources\.

1. From the KFP dashboard, terminate your pipeline runs if they do not exit properly by choosing **Terminate**\.

1. If the **Terminate** option doesn’t work, log in to your gateway node and manually terminate all the pods created by your pipeline run as follows:

   ```
   kubectl get pods -n kubeflow
   kubectl delete pods -n kubeflow <name-of-pipeline-pod>
   ```

1. Using your AWS account, log in to the SageMaker service\. Manually stop all training, batch transform, and HPO jobs\. Delete models, data buckets, and endpoints to avoid incurring any additional costs\. Terminating the pipeline runs does not stop the jobs in SageMaker\.