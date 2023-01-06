# Latest SageMaker Operators for Kubernetes<a name="kubernetes-sagemaker-operators-ack"></a>

This section is based on the latest version of SageMaker Operators for Kubernetes using AWS Controllers for Kubernetes \(ACK\)\.

**Important**  
If you are currently using [version `v1.2.2` or below of the original SageMaker Operators for Kubernetes](https://github.com/aws/amazon-sagemaker-operator-for-k8s), we recommend migrating your resources to the latest SageMaker Operators for Kubernetes, the [ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller) based on [AWS Controllers for Kubernetes \(ACK\)](https://aws-controllers-k8s.github.io/community/ )\.  
For information about the migration steps, see [Migrate resources to the latest Operators](kubernetes-sagemaker-operators-migrate.md)\. For answers to frequently asked questions regarding the end of support of the original version of SageMaker Operators for Kubernetes, see [Announcing the End of Support of the Original Version of SageMaker Operator for Kubernetes](kubernetes-sagemaker-operators-eos-announcement.md)

[The latest version of SageMaker Operators for Kubernetes](https://github.com/aws-controllers-k8s/sagemaker-controller) is based on [AWS Controllers for Kubernetes \(ACK\)](https://aws-controllers-k8s.github.io/community/ ), a framework for building Kubernetes custom controllers where each controller communicates with an AWS service API\. These controllers allow Kubernetes users to provision AWS resources like databases or message queues using the Kubernetes API\.

Use the following steps to install and use ACK to train, tune, and deploy machine learning models with Amazon SageMaker\.

**Topics**
+ [Install SageMaker Operators for Kubernetes](#kubernetes-sagemaker-operators-ack-install)
+ [Use SageMaker Operators for Kubernetes](#kubernetes-sagemaker-operators-ack-use)
+ [Reference](#kubernetes-sagemaker-operators-ack-reference)

## Install SageMaker Operators for Kubernetes<a name="kubernetes-sagemaker-operators-ack-install"></a>

To set up the latest available version of SageMaker Operators for Kubernetes, see the *Setup* section in [ Machine Learning with the ACK SageMaker Controller](https://aws-controllers-k8s.github.io/community/docs/tutorials/sagemaker-example/#setup)\.

## Use SageMaker Operators for Kubernetes<a name="kubernetes-sagemaker-operators-ack-use"></a>

For a tutorial on how to train a machine learning model with the ACK service controller for Amazon SageMaker using Amazon EKS, see [Machine Learning with the ACK SageMaker Controller](https://aws-controllers-k8s.github.io/community/docs/tutorials/sagemaker-example/)\.

For an autoscaling example, see [ Scale SageMaker Workloads with Application Auto Scaling](https://aws-controllers-k8s.github.io/community/docs/tutorials/autoscaling-example/)

## Reference<a name="kubernetes-sagemaker-operators-ack-reference"></a>

See also the [ACK service controller for Amazon SageMaker GitHub repository](https://github.com/aws-controllers-k8s/sagemaker-controller) or read [AWS Controllers for Kubernetes Documentation](https://aws-controllers-k8s.github.io/community/docs/community/overview/)\. 