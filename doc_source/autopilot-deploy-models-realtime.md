# Real\-time inferencing<a name="autopilot-deploy-models-realtime"></a>

Real\-time inference is ideal for inference workloads where you have real\-time, interactive, low latency requirements\. This section shows how you can use real\-time inferencing to obtain predictions interactively from your model\.

To deploy the model that produced the best validation metric in an Autopilot experiment, you have several options\. For example, when using Autopilot in SageMaker Studio, you can deploy the model automatically or manually\. You can also use SageMaker APIs to manually deploy an Autopilot model\. 

The following tabs show three options for deploying your model\. These instructions assume that you have already created a model in Autopilot\. If you don't have a model, see [Create an Amazon SageMaker Autopilot experiment](autopilot-automate-model-development-create-experiment.md)\. To see examples for each option, open each tab\.

## Deploy using the Autopilot User Interface \(UI\)<a name="autopilot-deploy-models-realtime-ui"></a>

The Autopilot UI contains helpful dropdown menus, toggles, tooltips, and more to help you navigate through model deployment\.
+ **Automatically**: To automatically deploy the best model from an Autopilot experiment to an endpoint, toggle the **Auto deploy** value to **Yes** when [creating the experiment](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-automate-model-development-create-experiment.html) in SageMaker Studio\. 
**Note**  
**Automatic deployment will fail if either the default resource quota or your customer quota for endpoint instances in a Region is too limited\.** In hyperparameter optimization \(HPO\) mode, you are required to have at least two ml\.m5\.2xlarge instances\. In ensembling mode, you are required to have at least one ml\.m5\.12xlarge instance\. If you encounter a failure related to quotas, you can [request a service limit increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) for SageMaker endpoint instances\.
+ **Manually**: To manually deploy the best model from an Autopilot experiment to an endpoint, toggle the **Auto deploy** value to **No** when creating the experiment in SageMaker Studio\. 

  After your Autopilot experiment has been created, select the model that you want to deploy under **Model name**\. Then select the orange **Deployment and advanced settings** button located on the right of the leaderboard\. This opens a new tab\.

  Configure the endpoint name, instance type, and other optional information\. Select the orange **Deploy model** to deploy to an endpoint\.

  Check the progress of the endpoint creation process in the [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) by navigating to the Endpoints section\. That section is located in the **Inference** dropdown menu in the navigation panel\. 

  After the endpoint status changes from **Creating** to **InService**, as shown below, return to Studio and invoke the endpoint\.  
![\[SageMaker console: Endpoints page to create an endpoint or check endpoint status.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-check-progress.PNG)

## Deploy using SageMaker APIs<a name="autopilot-deploy-models-api"></a>

You can also obtain real\-time inference by deploying your model using **API calls**\. This section shows the five steps of this process using AWS Command Line Interface \(AWS CLI\) code snippets\. 

For complete code examples for both AWS CLI commands and AWS SDK for Python \(Boto3\), open the tabs directly following these steps\.

1. **Obtain candidate definitions**

   Obtain the candidate container definitions from [InferenceContainers](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidate.html#sagemaker-Type-AutoMLCandidate-InferenceContainers)\. These candidate definitions are used to create a SageMaker model\. 

   The following example uses the [DescribeAutoMLJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html) API to obtain candidate definitions for the best model candidate\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker describe-auto-ml-job --auto-ml-job-name <job-name> --region <region>
   ```

1. **List candidates**

   The following example uses the [ListCandidatesForAutoMLJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidatesForAutoMLJob.html) API to list all candidates\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker list-candidates-for-auto-ml-job --auto-ml-job-name <job-name> --region <region>
   ```

1. **Create a SageMaker model**

   Use the container definitions from the previous steps to create a SageMaker model by using the [CreateModel](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker create-model --model-name '<your-custom-model-name>' \
                       --containers ['<container-definition1>, <container-definition2>, <container-definition3>]' \
                       --execution-role-arn '<execution-role-arn>' --region '<region>
   ```

1. **Create an endpoint configuration** 

   The following example uses the [CreateEndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) API to create an endpoint configuration\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker create-endpoint-config --endpoint-config-name '<your-custom-endpoint-config-name>' \
                       --production-variants '<list-of-production-variants>' \
                       --region '<region>'
   ```

1. **Create the endpoint** 

   The following AWS CLI example uses the [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API to create the endpoint\.

   ```
   aws sagemaker create-endpoint --endpoint-name '<your-custom-endpoint-name>' \
                       --endpoint-config-name '<endpoint-config-name-you-just-created>' \
                       --region '<region>'
   ```

   Check the progress of your endpoint deployment by using the [DescribeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html) API\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker describe-endpoint —endpoint-name '<endpoint-name>' —region <region>
   ```

   After the `EndpointStatus` changes to `InService`, the endpoint is ready to use for real\-time inference\.

1. **Invoke the endpoint** 

   The following command structure invokes the endpoint for real\-time inferencing\.

   ```
   aws sagemaker invoke-endpoint --endpoint-name '<endpoint-name>' \ 
                     --region '<region>' --body '<your-data>' [--content-type] '<content-type>' <outfile>
   ```

The following tabs contain complete code examples for deploying a model with  AWS SDK for Python \(Boto3\) or the AWS CLI\.

------
#### [ AWS SDK for Python \(Boto3\) ]

1. **Obtain the candidate definitions** by using the following code example\.

   ```
   import sagemaker 
   import boto3
   
   session = sagemaker.session.Session()
   
   sagemaker_client = boto3.client('sagemaker', region_name='us-west-2')
   job_name = 'test-auto-ml-job'
   
   describe_response = sm_client.describe_auto_ml_job(AutoMLJobName=job_name)
   # extract the best candidate definition from DescribeAutoMLJob response
   best_candidate = describe_response['BestCandidate']
   # extract the InferenceContainers definition from the caandidate definition
   inference_containers = best_candidate['InferenceContainers']
   ```

1. **Create the model** by using the following the code example\.

   ```
   # Create Model
   model_name = 'test-model' 
   sagemaker_role = 'arn:aws:iam:444455556666:role/sagemaker-execution-role'
   create_model_response = sagemaker_client.create_model(
      ModelName = model_name,
      ExecutionRoleArn = sagemaker_role,
      Containers = inference_containers 
   )
   ```

1. **Create the endpoint configuration** by using the following the code example\.

   ```
   endpoint_config_name = 'test-endpoint-config'
                                                           
   instance_type = 'ml.m5.2xlarge' 
   # for all supported instance types, see 
   # https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html#sagemaker-Type-ProductionVariant-InstanceType    # Create endpoint config
   
   endpoint_config_response = sagemaker_client.create_endpoint_config(
      EndpointConfigName=endpoint_config_name, 
      ProductionVariants=[
          {
              "VariantName": "variant1",
              "ModelName": model_name, 
              "InstanceType": instance_type,
              "InitialInstanceCount": 1
          }
      ]
   )
   
   print(f"Created EndpointConfig: {endpoint_config_response['EndpointConfigArn']}")
   ```

1. **Create the endpoint** and deploy the model with the following code example\.

   ```
   # create endpoint and deploy the model
   endpoint_name = 'test-endpoint'
   create_endpoint_response = sagemaker_client.create_endpoint(
                                               EndpointName=endpoint_name, 
                                               EndpointConfigName=endpoint_config_name)
   print(create_endpoint_response)
   ```

   **Check the status of creating the endpoint** by using the following the code example\.

   ```
   # describe endpoint creation status
   status = sagemaker_client.describe_endpoint(EndpointName=endpoint_name)["EndpointStatus"]
   ```

1. **Invoke the endpoint** for real\-time inferencing by using the following command structure\.

   ```
   # once endpoint status is InService, you can invoke the endpoint for inferencing
   if status == "InService":
     sm_runtime = boto3.Session().client('sagemaker-runtime')
     inference_result = sm_runtime.invoke_endpoint(EndpointName='test-endpoint', ContentType='text/csv', Body='1,2,3,4,class')
   ```

------
#### [ AWS Command Line Interface \(AWS CLI\) ]

1. **Obtain the candidate definitions** by using the following code example\.

   ```
   aws sagemaker describe-auto-ml-job --auto-ml-job-name 'test-automl-job' --region us-west-2
   ```

1. **Create the model** by using the following code example\.

   ```
   aws sagemaker create-model --model-name 'test-sagemaker-model'
   --containers '[{
       "Image": "348316444620.dkr.ecr.us-west-2.amazonaws.com/sagemaker-sklearn-automl:2.5-1-cpu-py3", DOC-EXAMPLE-BUCKET1
       "ModelDataUrl": "s3://DOC-EXAMPLE-BUCKET/output/model.tar.gz",
       "Environment": {
           "AUTOML_SPARSE_ENCODE_RECORDIO_PROTOBUF": "1",
           "AUTOML_TRANSFORM_MODE": "feature-transform",
           "SAGEMAKER_DEFAULT_INVOCATIONS_ACCEPT": "application/x-recordio-protobuf",
           "SAGEMAKER_PROGRAM": "sagemaker_serve",
           "SAGEMAKER_SUBMIT_DIRECTORY": "/opt/ml/model/code"
       }
   }, {
       "Image": "348316444620.dkr.ecr.us-west-2.amazonaws.com/sagemaker-xgboost:1.3-1-cpu-py3",
       "ModelDataUrl": "s3://DOC-EXAMPLE-BUCKET/output/model.tar.gz",
       "Environment": {
           "MAX_CONTENT_LENGTH": "20971520",
           "SAGEMAKER_DEFAULT_INVOCATIONS_ACCEPT": "text/csv",
           "SAGEMAKER_INFERENCE_OUTPUT": "predicted_label", 
           "SAGEMAKER_INFERENCE_SUPPORTED": "predicted_label,probability,probabilities" 
       }
   }, {
       "Image": "348316444620.dkr.ecr.us-west-2.amazonaws.com/sagemaker-sklearn-automl:2.5-1-cpu-py3", aws-region
       "ModelDataUrl": "s3://DOC-EXAMPLE-BUCKET/output/model.tar.gz", 
       "Environment": { 
           "AUTOML_TRANSFORM_MODE": "inverse-label-transform", 
           "SAGEMAKER_DEFAULT_INVOCATIONS_ACCEPT": "text/csv", 
           "SAGEMAKER_INFERENCE_INPUT": "predicted_label", 
           "SAGEMAKER_INFERENCE_OUTPUT": "predicted_label", 
           "SAGEMAKER_INFERENCE_SUPPORTED": "predicted_label,probability,labels,probabilities", 
           "SAGEMAKER_PROGRAM": "sagemaker_serve", 
           "SAGEMAKER_SUBMIT_DIRECTORY": "/opt/ml/model/code"
       } 
   }]' \
   --execution-role-arn 'arn:aws:iam::1234567890:role/sagemaker-execution-role' \ 
   --region 'us-west-2'
   ```

   For additional details, see [creating a model](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/create-model.html)\.

   The `create model` command will return a response in the following format\.

   ```
   {
       "ModelArn": "arn:aws:sagemaker:us-west-2:1234567890:model/test-sagemaker-model"
   }
   ```

1. **Create an endpoint configuration** by using the following code example\.

   ```
   aws sagemaker create-endpoint-config --endpoint-config-name 'test-endpoint-config' \
   --production-variants '[{"VariantName": "variant1", 
                           "ModelName": "test-sagemaker-model",
                           "InitialInstanceCount": 1,
                           "InstanceType": "ml.m5.2xlarge"
                          }]' \
   --region us-west-2
   ```

   The `create endpoint` configuration command will return a response in the following format\.

   ```
   {
       "EndpointConfigArn": "arn:aws:sagemaker:us-west-2:1234567890:endpoint-config/test-endpoint-config"
   }
   ```

1. **Create an endpoint** by using the following code example\.

   ```
   aws sagemaker create-endpoint --endpoint-name 'test-endpoint' \    
   --endpoint-config-name 'test-endpoint-config' \                 
   --region us-west-2
   ```

   The `create endpoint` command will return a response in the following format\.

   ```
   {
       "EndpointArn": "arn:aws:sagemaker:us-west-2:1234567890:endpoint/test-endpoint"
   }
   ```

   Check the progress of the endpoint deployment by using the following [describe\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/describe-endpoint.html) CLI code example\.

   ```
   aws sagemaker describe-endpoint --endpoint-name 'test-endpoint' --region us-west-2
   ```

   The previous progress check will return a response in the following format\.

   ```
   {
       "EndpointName": "test-endpoint",
       "EndpointArn": "arn:aws:sagemaker:us-west-2:1234567890:endpoint/test-endpoint",
       "EndpointConfigName": "test-endpoint-config",
       "EndpointStatus": "Creating",
       "CreationTime": 1660251167.595,
       "LastModifiedTime": 1660251167.595
   }
   ```

   After the `EndpointStatus` changes to `InService`, the endpoint is ready for use in real\-time inference\.

1. **Invoke the endpoint** for real\-time inferencing by using the following command structure\.

   ```
   aws sagemaker-runtime invoke-endpoint --endpoint-name 'test-endpoint' \
   --region 'us-west-2' \
   --body '1,51,3.5,1.4,0.2' \
   --content-type 'text/csv' \
   '/tmp/inference_output'
   ```

   For more options, see [invoking an endpoint](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker-runtime/invoke-endpoint.html)\.

------

## Deploy models from different accounts<a name="autopilot-deploy-models-realtime-across-accounts"></a>

You can deploy an Autopilot model from a different account than the original account that a model was generated in\. To implement cross\-account model deployment, this section shows how to do the following:   Grant permission to assume the role to the account you want to deploy from \(the generating account\)\.    Make a call to `DescribeAutoMLJob` from the deploying account to obtain model information\.    Grant access rights to the model artifacts from the generating account\.    

1. **Grant permission to the deploying account** 

   To assume the role in the generating account, you must grant permission to the deploying account\. This allows the deploying account to describe Autopilot jobs in the generating account\.

   The following example uses a generating account with a trusted `sagemaker-role` entity\. The example shows how to give a deploying account with the ID 111122223333 permission to assume the role of the generating account\.

   ```
   "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "Service": [
                       "sagemaker.amazonaws.com"
                   ],
                   "AWS": [ "111122223333"]
               },
               "Action": "sts:AssumeRole"
           }
   ```

   The new account with the ID 111122223333 can now assume the role for the generating account\. 

   Next, call the `DescribeAutoMLJob` API from the deploying account to obtain a description of the job created by the generating account\. 

   The following code example describes the model from the deploying account\.

   ```
   import sagemaker 
   import boto3
   session = sagemaker.session.Session()
   
   sts_client = boto3.client('sts')
   sts_client.assume_role
   
   role = 'arn:aws:iam::111122223333:role/sagemaker-role'
   role_session_name = "role-session-name"
   _assumed_role = sts_client.assume_role(RoleArn=role, RoleSessionName=role_session_name)
   
   credentials = _assumed_role["Credentials"]
   access_key = credentials["AccessKeyId"]
   secret_key = credentials["SecretAccessKey"]
   session_token = credentials["SessionToken"]
   
   session = boto3.session.Session()
           
   sm_client = session.client('sagemaker', region_name='us-west-2', 
                              aws_access_key_id=access_key,
                               aws_secret_access_key=secret_key,
                               aws_session_token=session_token)
   
   # now you can call describe automl job created in account A 
   
   job_name = "test-job"
   response= sm_client.describe_auto_ml_job(AutoMLJobName=job_name)
   ```

1. **Grant access to the deploying account** to the model artifacts in the generating account\.

   The deploying account only needs access to the model artifacts in the generating account to deploy it\. These are located in the [S3OutputPath](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLOutputDataConfig.html#sagemaker-Type-AutoMLOutputDataConfig-S3OutputPath) that was specified in the original `CreateAutoMLJob` API call during model generation\.

   To give the deploying account access to the model artifacts, choose one of the following options:

   1. [Give access](http://aws.amazon.com/premiumsupport/knowledge-center/cross-account-access-s3/) to the `ModelDataUrl` from the generating account to the deploying account\.

      Next, you need to give the deploying account permission to assume the role\. follow the [real\-time inferencing steps](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-deploy-models.html#autopilot-deploy-models-realtime) to deploy\. 

   1. [Copy model artifacts](http://aws.amazon.com/premiumsupport/knowledge-center/copy-s3-objects-account/) from the generating account's original [S3OutputPath](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLOutputDataConfig.html#sagemaker-Type-AutoMLOutputDataConfig-S3OutputPath) to the generating account\.

      To grant access to the model artifacts, you must define a `best_candidate` model and reassign model containers to the new account\. 

      The following example shows how to define a `best_candidate` model and reassign the `ModelDataUrl`\.

      ```
      best_candidate = automl.describe_auto_ml_job()['BestCandidate']
      
      # reassigning ModelDataUrl for best_candidate containers below
      new_model_locations = ['new-container-1-ModelDataUrl', 'new-container-2-ModelDataUrl', 'new-container-3-ModelDataUrl']
      new_model_locations_index = 0
      for container in best_candidate['InferenceContainers']:
          container['ModelDataUrl'] = new_model_locations[new_model_locations_index++]
      ```

      After this assignment of containers, follow the steps in [Deploy using SageMaker APIs](#autopilot-deploy-models-api) to deploy\.

To build a payload in real\-time inferencing, see the notebook example to [ define a test payload](http://aws.amazon.com/getting-started/hands-on/machine-learning-tutorial-automatically-create-models)\. To create the payload from a CSV file and invoke an endpoint, see the **Predict with your model** section in [Create a machine learning model automatically](http://aws.amazon.com/getting-started/hands-on/create-machine-learning-model-automatically-sagemaker-autopilot/#autopilot-cr-room)\.