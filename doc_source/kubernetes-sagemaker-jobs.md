# Using Amazon SageMaker Jobs<a name="kubernetes-sagemaker-jobs"></a>

To run an Amazon SageMaker job using the Operators for Kubernetes, you can either apply a YAML file or use the supplied Helm Charts\. 

**Important**  
The new version of SageMaker Operators for Kubernetes uses AWS Controllers for Kubernetes \(ACK\)\. For more information, see [Migrate resources to the new SageMaker Operators for Kubernetes](kubernetes-sagemaker-operators-migrate.md) or go directly to the [ACK documentation](https://aws-controllers-k8s.github.io/community/docs/community/overview/)\.

All sample operator jobs in the following tutorials use sample data taken from a public MNIST dataset\. In order to run these samples, download the dataset into your Amazon S3 bucket\. You can find the dataset in [Download the MNIST Dataset\.](https://docs.aws.amazon.com/sagemaker/latest/dg/ex1-preprocess-data-pull-data.html) 

**Topics**
+ [TrainingJob operator](trainingjob-operator.md)
+ [HyperParameterTuningJob operator](hyperparametertuningjobs-operator.md)
+ [BatchTransformJob operator](batchtransformjobs-operator.md)
+ [HostingDeployment operator](hosting-deployment-operator.md)
+ [ProcessingJob operator](kubernetes-processing-job-operator.md)
+ [HostingAutoscalingPolicy \(HAP\) Operator](kubernetes-hap-operator.md)