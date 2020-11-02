# API reference guide for Amazon SageMaker Autopilot<a name="autopilot-reference"></a>

Amazon SageMaker provides API reference documentation that describes all of the REST operations and data types used by Autopilot and a higher level [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) that you can use to create and manage AutoML jobs\. It also provides a command line interface \(CLI\), an AWS SDK for Python \(Boto\) for low\-level clients of SageMaker services, and SDKs for \.NET, C\+\+, Go, Java, JavaScript, PHP V3, and Ruby V3\. The following sections describe these Autopilot programming interfaces\.

**Topics**
+ [SageMaker API reference](#autopilot-api-reference)
+ [[Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)](#autopilot-sagemaker-python-sdk)
+ [AWS Command Line Interface \(CLI\)](#autopilot-cli)
+ [AWS SDK for Python \(Boto\)](#autopilot-aws-sdk-for-python-boto3)
+ [AWS SDK for \.NET](#autopilot-aws-sdk-for-dotnet)
+ [AWS SDK for C\+\+](#autopilot-aws-sdk-for-c-plus-plus)
+ [AWS SDK for Go](#autopilot-aws-sdk-for-go)
+ [AWS SDK for Java](#autopilot-aws-sdk-for-java)
+ [AWS SDK for JavaScript](#autopilot-aws-sdk-for-javascript)
+ [AWS SDK for PHP V3](#autopilot-aws-sdk-for-php)
+ [AWS SDK for Ruby V3](#autopilot-aws-sdk-for-ruby)

## SageMaker API reference<a name="autopilot-api-reference"></a>

This API provides HTTP service APIs for creating and managing Amazon SageMaker Autopilot resources\.

**Actions**
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListAutoMLJobs.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListAutoMLJobs.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidatesForAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidatesForAutoMLJob.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopAutoMLJob.html)

**Data Types**
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidate.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidate.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateStep.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateStep.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLContainerDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLContainerDefinition.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSource.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSource.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobArtifacts.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobArtifacts.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobCompletionCriteria.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobCompletionCriteria.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobSummary.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobSummary.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLOutputDataConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLOutputDataConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLS3DataSource.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLS3DataSource.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLSecurityConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLSecurityConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html)

For more information on the entire SageMaker REST API, see [API and SDK Reference](https://docs.aws.amazon.com/sagemaker/latest/dg/api-and-sdk-reference.html)\.

## [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)<a name="autopilot-sagemaker-python-sdk"></a>

This Python library provides several high\-level abstractions for working with SageMaker\. The following classes can be used to manage AutoML jobs\.
+ [https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.automl.AutoML](https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.automl.AutoML)
+ [https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.automl.AutoMLInput](https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.automl.AutoMLInput)
+ [https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.automl.AutoMLJob](https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.automl.AutoMLJob)
+ [https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.candidate_estimator.CandidateEstimator](https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.candidate_estimator.CandidateEstimator)
+ [https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.candidate_estimator.CandidateStep](https://sagemaker.readthedocs.io/en/stable/api/training/automl.html#sagemaker.automl.candidate_estimator.CandidateStep)

For more information how this Python SDK simplifies model training and deployment, see [Using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)](https://sagemaker.readthedocs.io/en/stable/overview.html)\.

## AWS Command Line Interface \(CLI\)<a name="autopilot-cli"></a>

The [AWS CLI](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html#cli-aws) provides APIs for creating and managing SageMaker resources\. Here are the [AWS CLI for Amazon SageMaker](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/index.html) Autopilot commands\.
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/create-auto-ml-job.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/create-auto-ml-job.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/describe-auto-ml-job.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/describe-auto-ml-job.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/list-auto-ml-jobs.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/list-auto-ml-jobs.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/list-candidates-for-auto-ml-job.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/list-candidates-for-auto-ml-job.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/stop-auto-ml-job.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/stop-auto-ml-job.html)

## AWS SDK for Python \(Boto\)<a name="autopilot-aws-sdk-for-python-boto3"></a>

Boto is the Amazon Web Services \(AWS\) SDK for Python\. It enables Python developers to create, configure, and manage AWS services such as SageMaker\. Boto provides a low\-level [Client](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#client) API that maps to the underlying SageMaker service API\. Here is a list of the methods used to manage AutoML jobs with the Client class\.
+ [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_auto_ml_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_auto_ml_job)
+ [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.describe_auto_ml_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.describe_auto_ml_job)
+ [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.list_auto_ml_jobs](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.list_auto_ml_jobs)
+ [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.list_candidates_for_auto_ml_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.list_candidates_for_auto_ml_job)
+ [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.stop_auto_ml_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.stop_auto_ml_job)

## AWS SDK for \.NET<a name="autopilot-aws-sdk-for-dotnet"></a>

The \.NET SDK enables developers to create, configure, and manage AWS services such as SageMaker\. The API maps to the underlying SageMaker service API\. Here is a list of the methods used to manage AutoML jobs with the Client class\.
+ [https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerCreateAutoMLJobCreateAutoMLJobRequest.html](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerCreateAutoMLJobCreateAutoMLJobRequest.html)
+ [https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerDescribeAutoMLJobDescribeAutoMLJobRequest.html](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerDescribeAutoMLJobDescribeAutoMLJobRequest.html)
+ [https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerListAutoMLJobsListAutoMLJobsRequest.html](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerListAutoMLJobsListAutoMLJobsRequest.html)
+ [https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerListCandidatesForAutoMLJobListCandidatesForAutoMLJobRequest.html](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerListCandidatesForAutoMLJobListCandidatesForAutoMLJobRequest.html)
+ [https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerStopAutoMLJobStopAutoMLJobRequest.html](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SageMaker/MSageMakerStopAutoMLJobStopAutoMLJobRequest.html)

## AWS SDK for C\+\+<a name="autopilot-aws-sdk-for-c-plus-plus"></a>

The C\+\+ SDK enables developers to create, configure, and manage AWS services such as SageMaker\. The API maps to the underlying SageMaker service API\. For information on the methods used to manage AutoML jobs with the Client class, see [https://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_sage_maker_1_1_sage_maker_client.html](https://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_sage_maker_1_1_sage_maker_client.html)\.

## AWS SDK for Go<a name="autopilot-aws-sdk-for-go"></a>

The Go SDK enables developers to create, configure, and manage AWS services such as SageMaker\. The API maps to the underlying SageMaker service API\. Here is a list of the methods used to manage AutoML jobs with the Client class\.
+ [https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.CreateAutoMLJob](https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.CreateAutoMLJob)
+ [https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.DescribeAutoMLJob](https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.DescribeAutoMLJob)
+ [https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.ListAutoMLJobs](https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.ListAutoMLJobs)
+ [https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.ListCandidatesForAutoMLJob](https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.ListCandidatesForAutoMLJob)
+ [https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.StopAutoMLJob](https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/#SageMaker.StopAutoMLJob)

## AWS SDK for Java<a name="autopilot-aws-sdk-for-java"></a>

The Java SDK enables developers to create, configure, and manage AWS services such as SageMaker\. The API maps to the underlying SageMaker service API\. Here is a list of the methods used to manage AutoML jobs with the Client class\.
+ [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#createAutoMLJob-com.amazonaws.services.sagemaker.model.CreateAutoMLJobRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#createAutoMLJob-com.amazonaws.services.sagemaker.model.CreateAutoMLJobRequest-)
+ [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#describeAutoMLJob-com.amazonaws.services.sagemaker.model.DescribeAutoMLJobRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#describeAutoMLJob-com.amazonaws.services.sagemaker.model.DescribeAutoMLJobRequest-)
+ [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#listAutoMLJobs-com.amazonaws.services.sagemaker.model.ListAutoMLJobsRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#listAutoMLJobs-com.amazonaws.services.sagemaker.model.ListAutoMLJobsRequest-)
+ [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#listCandidatesForAutoMLJob-com.amazonaws.services.sagemaker.model.ListCandidatesForAutoMLJobRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#listCandidatesForAutoMLJob-com.amazonaws.services.sagemaker.model.ListCandidatesForAutoMLJobRequest-)
+ [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#stopAutoMLJob-com.amazonaws.services.sagemaker.model.StopAutoMLJobRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html#stopAutoMLJob-com.amazonaws.services.sagemaker.model.StopAutoMLJobRequest-)

## AWS SDK for JavaScript<a name="autopilot-aws-sdk-for-javascript"></a>

The JavaScript SDK enables developers to create, configure, and manage AWS services such as SageMaker\. The API maps to the underlying SageMaker service API\. Here is a list of the methods used to manage AutoML jobs with the Client class\.
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#createAutoMLJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#createAutoMLJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#describeAutoMLJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#describeAutoMLJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#listAutoMLJobs-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#listAutoMLJobs-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#listCandidatesForAutoMLJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#listCandidatesForAutoMLJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#stopAutoMLJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html#stopAutoMLJob-property)

## AWS SDK for PHP V3<a name="autopilot-aws-sdk-for-php"></a>

The PHP V3 SDK enables developers to create, configure, and manage AWS services such as SageMaker\. The API maps to the underlying SageMaker service API\. Here is a list of the methods used to manage AutoML jobs with the Client class\.
+ [https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#createautomljob](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#createautomljob)
+ [https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#describeautomljob](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#describeautomljob)
+ [https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#listautomljobs](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#listautomljobs)
+ [https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#listcandidatesforautomljob](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#listcandidatesforautomljob)
+ [https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#stopautomljob](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html#stopautomljob)

## AWS SDK for Ruby V3<a name="autopilot-aws-sdk-for-ruby"></a>

The Ruby V3 SDK enables developers to create, configure, and manage AWS services such as SageMaker\. The API maps to the underlying SageMaker service API\. Here is a list of the methods used to manage AutoML jobs with the Client class\.
+ [https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#create_auto_ml_job-instance_method](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#create_auto_ml_job-instance_method)
+ [https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#describe_auto_ml_job-instance_method](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#describe_auto_ml_job-instance_method)
+ [https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#list_auto_ml_jobs-instance_method](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#list_auto_ml_jobs-instance_method)
+ [https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#list_candidates_for_auto_ml_job-instance_method](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#list_candidates_for_auto_ml_job-instance_method)
+ [https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#stop_auto_ml_job-instance_method](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker/Client.html#stop_auto_ml_job-instance_method)