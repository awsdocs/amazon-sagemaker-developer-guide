# Using Amazon SageMaker Jobs<a name="using-amazon-sagemaker-jobs"></a>

To run an Amazon SageMaker job using the Operators for Kubernetes, you can either apply a YAML file or use the supplied Helm Charts\. 

All sample operator jobs in the following tutorials use sample data taken from a public MNIST dataset\. In order to run these samples, download the dataset into your Amazon S3 bucket\. You can find the dataset in [Download the MNIST Dataset\.](https://docs.aws.amazon.com/sagemaker/latest/dg/ex1-preprocess-data-pull-data.html) 

**Topics**
+ [TrainingJob operator](trainingjob-operator.md)
+ [HyperParameterTuningJob operator](hyperparametertuningjobs-operator.md)
+ [BatchTransformJob operator](batchtransformjobs-operator.md)
+ [HostingDeployment operator](hosting-deployment-operator.md)
+ [ProcessingJob operator](kubernetes-processing-job-operator.md)
+ [HostingAutoscalingPolicy \(HAP\) Operator](kubernetes-hap-operator.md)