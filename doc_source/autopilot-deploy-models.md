# Amazon SageMaker Autopilot model deployment<a name="autopilot-deploy-models"></a>

To deploy the model that produced the best validation metric in an Autopilot experiment, you have several options\. When using Autopilot in SageMaker Studio, you can deploy the model automatically or manually\. When working in another development, you can call Autopilot APIs directly to deploy a model\.
+ **Automatically**: To automatically deploy the best model from an Autopilot experiment to an endpoint, accept the default **Auto deploy** value **On** when creating the experiment in SageMaker Studio\.  
![\[Select Decide to use automatic deployment..\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-auto-deploy.png)
+ **Manually**: To manually deploy the best model from an Autopilot experiment to an endpoint, set the **Auto deploy** value to **Off** when creating the experiment in SageMaker Studio\.  
![\[Select Decide to use automatic deployment..\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-manual-deploy.png)
+ **API calls**: Make the following series of API calls:

  1. [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html)

  1. [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html)

  1. [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidatesForAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidatesForAutoMLJob.html)

  1. [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html)

  1. [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)

  1. [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html)

The automatic deployment for the results of an experiment in SageMaker Studio calls the six APIs listed in this last option by default\. For information on how to create an experiment, see [Create an Amazon SageMaker Autopilot experiment](autopilot-automate-model-development-create-experiment.md)\.

**Note**  
To avoid incurring unnecessary charges, delete the endpoints and resources created when deploying the model after they are no longer needed\.