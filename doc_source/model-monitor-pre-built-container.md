# Amazon SageMaker Model Monitor Pre\-built Container<a name="model-monitor-pre-built-container"></a>

SageMaker provides a built\-in container sagemaker\-model\-monitor\-analyzer that provides you with a range of model monitoring capabilities, including constraint suggestion, statistics generation, constraint validation against a baseline, and emitting Amazon CloudWatch metrics\. This container is based on Spark and is built with [Deequ](https://github.com/awslabs/deequ)\. The prebuilt container for SageMaker Model Monitor can be accessed at:

`<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/sagemaker-model-monitor-analyzer`

For example: `159807026194.dkr.ecr.us-west-2.amazonaws.com/sagemaker-model-monitor-analyzer`

The following table lists the supported values for account IDs and corresponding AWS Region names\.


| ACCOUNT\_ID | REGION\_NAME | 
| --- | --- | 
| 156813124566 | us\-east\-1 | 
| 777275614652 | us\-east\-2 | 
| 890145073186 | us\-west\-1 | 
| 159807026194 | us\-west\-2 | 
| 001633400207 | ap\-east\-1 | 
| 574779866223 | ap\-northeast\-1 | 
| 709848358524 | ap\-northeast\-2 | 
| 126357580389 | ap\-south\-1 | 
| 245545462676 | ap\-southeast\-1 | 
| 563025443158 | ap\-southeast\-2 | 
| 536280801234 | ca\-central\-1 | 
| 453000072557 | cn\-north\-1 | 
| 453252182341 | cn\-northwest\-1 | 
| 048819808253 | eu\-central\-1 | 
| 895015795356 | eu\-north\-1 | 
| 468650794304 | eu\-west\-1 | 
| 749857270468 | eu\-west\-2 | 
| 680080141114 | eu\-west\-3 | 
| 607024016150 | me\-south\-1 | 
| 539772159869 | sa\-east\-1 | 
| 362178532790 | us\-gov\-west\-1 | 

To write your own analysis container, see the container contract described in [Customize Monitoring](model-monitor-custom-monitoring-schedules.md)\.