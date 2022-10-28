# SageMaker Components for Kubeflow Pipelines<a name="kubernetes-sagemaker-components-for-kubeflow-pipelines"></a>

This document outlines how to use SageMaker Components for Kubeflow Pipelines \(KFP\)\. With these pipeline components, you can create and monitor native SageMaker training, tuning, endpoint deployment, and batch transform jobs from your Kubeflow Pipelines\. By running Kubeflow Pipeline jobs on SageMaker, you move data processing and training jobs from the Kubernetes cluster to SageMaker’s machine learning\-optimized managed service\. This document assumes prior knowledge of Kubernetes and Kubeflow\. 

**Topics**
+ [What is Kubeflow Pipelines?](#what-is-kubeflow-pipelines)
+ [Kubeflow Pipeline components](#kubeflow-pipeline-components)
+ [IAM permissions](#iam-permissions)
+ [Converting Pipelines to use SageMaker](#converting-pipelines-to-use-amazon-sagemaker)
+ [Using SageMaker Components](kubernetes-sagemaker-components-tutorials.md)

## What is Kubeflow Pipelines?<a name="what-is-kubeflow-pipelines"></a>

Kubeflow Pipelines \(KFP\) is a platform for building and deploying portable, scalable machine learning \(ML\) workflows based on Docker containers\. The Kubeflow Pipelines platform consists of the following: 
+  A user interface \(UI\) for managing and tracking experiments, jobs, and runs\. 
+  An engine \(Argo\) for scheduling multi\-step ML workflows\. 
+  A AWS SDK for Python \(Boto3\) SDK for defining and manipulating pipelines and components\. 
+  Notebooks for interacting with the system using the SDK\. 

A pipeline is a description of an ML workflow expressed as a [directed acyclic graph](https://www.kubeflow.org/docs/pipelines/concepts/graph/) as shown in the following diagram\.  Every step in the workflow is expressed as a Kubeflow Pipeline [component](https://www.kubeflow.org/docs/pipelines/overview/concepts/component/), which is a AWS SDK for Python \(Boto3\) module\. 

If your data has been preprocessed, the standard pipeline can take a subset of the data and run hyperparameter optimization of your model\. The pipeline then trains a model with the full dataset using the optimal hyperparameters\. This model is used for both batch inference and endpoint creation\. 

For more information on Kubeflow Pipelines, see the [Kubeflow Pipelines documentation](https://www.kubeflow.org/docs/pipelines/)\. 

## Kubeflow Pipeline components<a name="kubeflow-pipeline-components"></a>

A Kubeflow Pipeline component is a set of code used to execute one step of a Kubeflow pipeline\. Components are represented by a AWS SDK for Python \(Boto3\) module built into a Docker image\. These components make it fast and easy to write pipelines for experimentation and production environments without having to interact with the underlying Kubernetes infrastructure\.

### SageMaker Components for Kubeflow Pipelines versions<a name="kubeflow-pipeline-components-versions"></a>

SageMaker Components for Kubeflow Pipelines come in two versions\. Each version leverages a different backend to create and manage resources on SageMaker\. 
+ The SageMaker Components for Kubeflow Pipelines version 1 \(v1\.x or below\) use **[Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html)** \(AWS SDK for AWS SDK for Python \(Boto3\)\) as backend\. 
+ The version 2 \(v2\.0\.0\-alpha2 and above\) of SageMaker Components for Kubeflow Pipelines use [ SageMaker Operator for Kubernetes \(ACK\)](https://github.com/aws-controllers-k8s/sagemaker-controller)\. 

  AWS introduced [ACK](https://aws-controllers-k8s.github.io/community/) to facilitate a Kubernetes\-native way of managing AWS Cloud resources\. ACK includes a set of AWS service\-specific controllers, one of which is the SageMaker controller\. The SageMaker controller makes it easier for machine learning developers and data scientists using Kubernetes as their control plane to train, tune, and deploy machine learning \(ML\) models in SageMaker\. For more information, see [SageMaker Operators for Kubernetes]( https://aws-controllers-k8s.github.io/community/docs/tutorials/sagemaker-example/) 

Both versions of the SageMaker Components for Kubeflow Pipelines are supported\. However, the version 2 provides some additional advantages\. In particular, it offers: 

1. A consistent experience to manage your SageMaker resources from any application; whether you are using Kubeflow pipelines, or Kubernetes CLI \(`kubectl`\) or other Kubeflow applications such as Notebooks\. 

1. The flexibility to manage and monitor your SageMaker resources outside of the Kubeflow pipeline workflow\. 

1. Zero setup time to use the components if you deployed the full [Kubeflow on AWS](https://awslabs.github.io/kubeflow-manifests/docs/about/) release since the SageMaker Operator is part of its deployment\. 

### What do SageMaker Components for Kubeflow Pipelines provide?<a name="what-doamazon-sagemaker-components-for-kubeflow-pipelines-provide"></a>

SageMaker Components for Kubeflow Pipelines offer an alternative to launching your compute\-intensive jobs from SageMaker\. The components integrate SageMaker with the portability and orchestration of Kubeflow Pipelines\. Using the SageMaker Components for Kubeflow Pipelines \(KFP\), you can create and monitor your SageMaker resources as part of a Kubeflow Pipelines workflow\. Each of the jobs in your pipelines runs on SageMaker instead of the local Kubernetes cluster\. The job parameters, status, logs, and outputs from SageMaker are still accessible from the Kubeflow Pipelines UI\. 

 The following SageMaker components have been created to integrate six key SageMaker features into your ML workflows\. You can create a Kubeflow Pipeline built entirely using these components, or integrate individual components into your workflow as needed\. Alternatively, you can find all [SageMaker Components for Kubeflow Pipelines in GitHub\.](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker#versioning) 

There is no additional charge for using SageMaker Components for Kubeflow Pipelines\. You incur charges for any SageMaker resources you use through these components\. 

#### Training components<a name="training-components"></a>

 **Processing** 

The Processing component enables you to submit processing jobs to SageMaker directly from a Kubeflow Pipelines workflow\. For more information, see [SageMaker Processing Kubeflow Pipeline component version 1](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker/process)\. 

**Training** 

The Training component allows you to submit SageMaker Training jobs directly from a Kubeflow Pipelines workflow\. For more information, see [SageMaker Training Kubeflow Pipelines component version 2](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker/TrainingJob)\. For information about Version 1 of the Training component see [SageMaker Training Kubeflow Pipelines component version 1](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker/train)\. 

 **Hyperparameter Optimization** 

The Hyperparameter Optimization component enables you to submit hyperparameter tuning jobs to SageMaker directly from a Kubeflow Pipelines workflow\. For more information, see [SageMaker hyperparameter optimization Kubeflow Pipeline component version 1](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker/hyperparameter_tuning)\. 

#### Inference components<a name="inference-components"></a>

 **Hosting Deploy** 

The Deploy component enables you to deploy a model in SageMaker Hosting from a Kubeflow Pipelines workflow\. For more information, see [SageMaker Hosting Services \- Create Endpoint Kubeflow Pipeline component version 1](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker/deploy)\. 

 **Batch Transform component** 

The Batch Transform component enables you to run inference jobs for an entire dataset in SageMaker from a Kubeflow Pipelines workflow\. For more information, see [SageMaker Batch Transform Kubeflow Pipeline component version 1](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker/batch_transform)\. 

#### Ground Truth components<a name="ground-truth-components"></a>

 **Ground Truth** The Ground Truth component enables you to submit SageMaker Ground Truth labeling jobs directly from a Kubeflow Pipelines workflow\. For more information, see [SageMaker Ground Truth Kubeflow Pipelines component version 1](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker/ground_truth)\. 

 **Workteam** 

The Workteam component enables you to create SageMaker private workteam jobs directly from a Kubeflow Pipelines workflow\. For more information, see [SageMaker create private workteam Kubeflow Pipelines component version 1](https://github.com/kubeflow/pipelines/tree/master/components/aws/sagemaker/workteam)\. 

## IAM permissions<a name="iam-permissions"></a>

Deploying Kubeflow Pipelines with SageMaker components requires the following three levels of IAM permissions: 
+ An IAM user/role to access your AWS account \(**your\_credentials**\)\. Note: You don’t need this at all if you already have access to KFP web UI and have your input data in Amazon S3, or if you already have an Amazon Elastic Kubernetes Service \(Amazon EKS\) cluster with KFP\. 

  You use this user/role from your gateway node, which can be your local machine or a remote instance, to: 
  + Create an Amazon EKS cluster and install KFP
  + Create IAM roles/users
  + Create Amazon S3 buckets for your sample input data

  The IAM user/role needs the following permissions:
  + CloudWatchLogsFullAccess 
  + [AWSCloudFormationFullAccess](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess) 
  + [https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess) 
  + IAMFullAccess
  + AmazonS3FullAccess
  + AmazonEC2FullAccess
  + AmazonEKSAdminPolicy \(Create this policy using the schema from [Amazon EKS Identity\-Based Policy Examples](https://docs.aws.amazon.com/eks/latest/userguide/security_iam_id-based-policy-examples.html)\) 
+ An IAM role used by pipeline pods \(**kfp\-example\-pod\-role**\) or the SageMaker Operator for Kubernetes controller pod to access SageMaker\. This permission is used to create and monitor SageMaker jobs\. Note: You can limit permissions to the KFP and controller pods by creating and attaching your own custom policy\.

  The role needs the following permission: 
  + AmazonSageMakerFullAccess 
+ An IAM role used by SageMaker jobs to access resources such as Amazon S3 and Amazon ECR etc\. \(**kfp\-example\-sagemaker\-execution\-role**\)\. 

  Your SageMaker jobs use this role to: 
  + Access SageMaker resources 
  + Input Data from Amazon S3 
  + Store your output model to Amazon S3 

  The role needs the following permissions: 
  + AmazonSageMakerFullAccess 
  + AmazonS3FullAccess 

 These are all the IAM users/roles you need to run KFP components for SageMaker\. 

When you have run the components and have created the SageMaker endpoint, you also need a role with the `sagemaker:InvokeEndpoint` permission to query inference endpoints\. 

## Converting Pipelines to use SageMaker<a name="converting-pipelines-to-use-amazon-sagemaker"></a>

You can convert an existing pipeline to use SageMaker by porting your generic AWS SDK for Python \(Boto3\) [processing containers](https://docs.aws.amazon.com/sagemaker/latest/dg/amazon-sagemaker-containers.html) and [training containers](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo.html)\. If you are using SageMaker for inference, you also need to attach IAM permissions to your cluster and convert an artifact to a model\. 