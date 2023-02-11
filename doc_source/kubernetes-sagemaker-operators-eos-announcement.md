# Announcing the End of Support of the Original Version of SageMaker Operator for Kubernetes<a name="kubernetes-sagemaker-operators-eos-announcement"></a>

This page announces the end of support of [SageMaker Operator for Kubernetes](https://github.com/aws/amazon-sagemaker-operator-for-k8s) in its original version and provides answers to frequently asked questions as well as migration information to the latest fully supported version of the SageMaker Operator for Kubernetes, the [ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller) based on [AWS Controllers for Kubernetes \(ACK\)](https://aws-controllers-k8s.github.io/community/ )\. For general information on SageMaker Operator for Kubernetes, see [Latest SageMaker Operators for Kubernetes](kubernetes-sagemaker-operators-ack.md)\. 

## End of Support Frequently Asked Questions<a name="kubernetes-sagemaker-operators-eos-faq"></a>

**Topics**
+ [Why are we ending support for SageMaker Operator for Kubernetes in its original version?](#kubernetes-sagemaker-operators-eos-faq-why)
+ [Where can I find more information about the new SageMaker Operator for Kubernetes and ACK?](#kubernetes-sagemaker-operators-eos-faq-more)
+ [What does End\-of\-Support \(EOS\) mean?](#kubernetes-sagemaker-operators-eos-faq-definition)
+ [How can I migrate my workload to the new SageMaker Operator for Kubernetes for training and inference?](#kubernetes-sagemaker-operators-eos-faq-how)
+ [Which version of ACK should I migrate to?](#kubernetes-sagemaker-operators-eos-faq-version)
+ [Are the initial SageMaker Operators for Kubernetes and the new Operators \(ACK service controller for Amazon SageMaker\) functionally equivalent?](#kubernetes-sagemaker-operators-eos-faq-parity)

### Why are we ending support for SageMaker Operator for Kubernetes in its original version?<a name="kubernetes-sagemaker-operators-eos-faq-why"></a>

SageMaker Operators for Kubernetes has a new fully supported version, the [ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller) based on [AWS Controllers for Kubernetes](https://aws-controllers-k8s.github.io/community/ ) \(ACK\), a framework for exposing AWS services via a Kubernetes operator\. We are therefore announcing the end of support \(EOS\) of the [previous version of SageMaker Operator for Kubernetes](https://github.com/aws/amazon-sagemaker-operator-for-k8s) which does not use AWS ACK framework\. The support will end on **Feb 15, 2023** along with [Amazon Elastic Kubernetes Service Kubernetes 1\.21](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions.html#kubernetes-release-calendar)\.

ACK is a community\-driven project optimized for production, standardizing the way to expose AWS services via a Kubernetes operator\. For more information, see [ACK history and tenets](https://aws-controllers-k8s.github.io/community/docs/community/background/)\.

### Where can I find more information about the new SageMaker Operator for Kubernetes and ACK?<a name="kubernetes-sagemaker-operators-eos-faq-more"></a>
+ For more information on the new SageMaker Operator for Kubernetes, see [ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller) GitHub Repository or read [AWS Controllers for Kubernetes Documentation](https://aws-controllers-k8s.github.io/community/docs/community/overview/)\.
+ For a tutorial on how to train a machine learning model with the ACK service controller for Amazon SageMaker using Amazon EKS, see [SageMaker example](https://aws-controllers-k8s.github.io/community/docs/tutorials/sagemaker-example/)\.

  For an autoscaling example, see [ Scale SageMaker Workloads with Application Auto Scaling](https://aws-controllers-k8s.github.io/community/docs/tutorials/autoscaling-example/)
+ For information on the AWS Controller for Kubernetes the new Operator is based on, see the [AWS Controllers for Kubernetes](https://aws-controllers-k8s.github.io/community/) \(ACK\) documentation\.
+ For a list of supported SageMaker resources, see [ACK API Reference](https://aws-controllers-k8s.github.io/community/reference/)\.

### What does End\-of\-Support \(EOS\) mean?<a name="kubernetes-sagemaker-operators-eos-faq-definition"></a>

While users can continue to use their current operators, we are no longer developing new features, nor will we release any patches or security updates for any issues found\. `v1.2.2` is the last release of [SageMaker Operator for Kubernetes](https://github.com/aws/amazon-sagemaker-operator-for-k8s/tree/master)\. Users should migrate their workloads to use the newer version of SageMaker Operator, the [ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller)\.

### How can I migrate my workload to the new SageMaker Operator for Kubernetes for training and inference?<a name="kubernetes-sagemaker-operators-eos-faq-how"></a>

For information about migrating resources from the old to the new SageMaker Operators for Kubernetes, follow [Migrate resources to the latest Operators](kubernetes-sagemaker-operators-migrate.md)\.

### Which version of ACK should I migrate to?<a name="kubernetes-sagemaker-operators-eos-faq-version"></a>

Users should migrate to the [most recent released version of the ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller/tags)\.

### Are the initial SageMaker Operators for Kubernetes and the new Operators \(ACK service controller for Amazon SageMaker\) functionally equivalent?<a name="kubernetes-sagemaker-operators-eos-faq-parity"></a>

Yes, they are at feature parity\.

A few highlights of the main notable differences between the two versions:
+  The Custom Resources Definitions \(CRD\) used by the ACK\-based SageMaker Operators for Kubernetes follow the AWS API definition making it incompatible with the Custom Resources specifications from the SageMaker Operators for Kubernetes in its initial version\. Refer to the [CRDs](https://github.com/aws-controllers-k8s/sagemaker-controller/tree/main/helm/crds) in the new controller or use the migration guide to adopt the resources and use the new controller\. 
+  The `Hosting Autoscaling` policy is no longer part of the new SageMaker Operators for Kubernetes and has been migrated to the [Application autoscaling](https://github.com/aws-controllers-k8s/applicationautoscaling-controller) ACK controller\. To learn about the Application autoscaling controller to configure autoscaling on SageMaker Endpoints, follow this [autoscaling example](https://aws-controllers-k8s.github.io/community/docs/tutorials/autoscaling-example/)\. 
+  The `HostingDeployment` resource was used to create Models, Endpoint Configurations and Endpoints in one CRD\. The new SageMaker Operators for Kubernetes has a separate CRD for each of these resources\. 