# Migrate resources to the latest Operators<a name="kubernetes-sagemaker-operators-migrate"></a>

We are stopping the development and technical support of [SageMaker Operators for Kubernetes](https://github.com/aws/amazon-sagemaker-operator-for-k8s/tree/master) in its original version\. If you are currently using version `v1.2.2` or below of the original version of SageMaker Operators for Kubernetes, we recommend migrating your resources to the latest version of the SageMaker Operators for Kubernetes, the [ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller) based on [AWS Controllers for Kubernetes](https://aws-controllers-k8s.github.io/community/)\.

For answers to frequently asked questions regarding the end of support of the original version of SageMaker Operators for Kubernetes, see [Announcing the End of Support of the Original Version of SageMaker Operator for Kubernetes](kubernetes-sagemaker-operators-eos-announcement.md)\.

Use the following steps to migrate your resources and use ACK to train, tune, and deploy machine learning models with Amazon SageMaker\.

**Note**  
The latest SageMaker Operators for Kubernetes are not backwards compatible\.

**Topics**
+ [Prerequisites](#migrate-resources-to-new-operators-prerequisites)
+ [Adopt resources](#migrate-resources-to-new-operators-steps)
+ [Clean up old resources](#migrate-resources-to-new-operators-cleanup)
+ [Use the new SageMaker Operators for Kubernetes](#migrate-resources-to-new-operators-tutorials)

## Prerequisites<a name="migrate-resources-to-new-operators-prerequisites"></a>

To successfully migrate resources to the latest SageMaker Operators for Kubernetes, you must:

1. Install the latest SageMaker Operators for Kubernetes\. See [Setup](https://aws-controllers-k8s.github.io/community/docs/tutorials/sagemaker-example/#setup) in *Machine Learning with the ACK SageMaker Controller* for step\-by\-step instructions\.

1. If you are using [HostingAutoscalingPolicy resources](#migrate-resources-to-new-operators-hap), install the new Application Auto Scaling Operator\. See [Setup](https://aws-controllers-k8s.github.io/community/docs/tutorials/autoscaling-example/#setup) in *Scale SageMaker Workloads with Application Auto Scaling* for step\-by\-step instructions\. This step is optional if you are not using HostingAutoScalingPolicy resources\.

If permissions are configured correctly, then the ACK SageMaker service controller can determine the specification and status of the AWS resource and reconcile the resource as if the ACK controller originally created it\. 

## Adopt resources<a name="migrate-resources-to-new-operators-steps"></a>

The new SageMaker Operators for Kubernetes provide the ability to adopt resources that were not originally created by the ACK service controller\. For more information, see [Adopt Existing AWS Resources](https://aws-controllers-k8s.github.io/community/docs/user-docs/adopted-resource/) in the ACK documentation\.

The following steps show how the new SageMaker Operators for Kubernetes can adopt an existing SageMaker endpoint\. Save the following sample to a file named `adopt-endpoint-sample.yaml`\. 

```
apiVersion: services.k8s.aws/v1alpha1
kind: AdoptedResource
metadata:
  name: adopt-endpoint-sample
spec:  
  aws:
    # resource to adopt, not created by ACK
    nameOrID: xgboost-endpoint
  kubernetes:
    group: sagemaker.services.k8s.aws
    kind: Endpoint
    metadata:
      # target K8s CR name
      name: xgboost-endpoint
```

Submit the custom resource \(CR\) using `kubectl apply`:

```
kubectl apply -f adopt-endpoint-sample.yaml
```

Use `kubectl describe` to check the status conditions of your adopted resource\.

```
kubectl describe adoptedresource adopt-endpoint-sample
```

Verify that the `ACK.Adopted` condition is `True`\. The output should look similar to the following example:

```
---
kind: AdoptedResource
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: '{"apiVersion":"services.k8s.aws/v1alpha1","kind":"AdoptedResource","metadata":{"annotations":{},"name":"xgboost-endpoint","namespace":"default"},"spec":{"aws":{"nameOrID":"xgboost-endpoint"},"kubernetes":{"group":"sagemaker.services.k8s.aws","kind":"Endpoint","metadata":{"name":"xgboost-endpoint"}}}}'
  creationTimestamp: '2021-04-27T02:49:14Z'
  finalizers:
  - finalizers.services.k8s.aws/AdoptedResource
  generation: 1
  name: adopt-endpoint-sample
  namespace: default
  resourceVersion: '12669876'
  selfLink: "/apis/services.k8s.aws/v1alpha1/namespaces/default/adoptedresources/adopt-endpoint-sample"
  uid: 35f8fa92-29dd-4040-9d0d-0b07bbd7ca0b
spec:
  aws:
    nameOrID: xgboost-endpoint
  kubernetes:
    group: sagemaker.services.k8s.aws
    kind: Endpoint
    metadata:
      name: xgboost-endpoint
status:
  conditions:
  - status: 'True'
    type: ACK.Adopted
```

Check that your resource exists in your cluster:

```
kubectl describe endpoints.sagemaker xgboost-endpoint
```

### HostingAutoscalingPolicy resources<a name="migrate-resources-to-new-operators-hap"></a>

The HostingAutoscalingPolicy \(HAP\) resource consists of multiple Application Auto Scaling resources: `ScalableTarget` and `ScalingPolicy`\. When adopting a HAP resource with ACK, you need to first install the [Application Auto Scaling controller](https://github.com/aws-controllers-k8s/applicationautoscaling-controller)\. To adopt HAP resources, you need to adopt both `ScalableTarget` and `ScalingPolicy` resources\. You can find the resource indentifier for these resources in the status of the HostingAutoscalingPolicy resource \(`status.ResourceIDList`\)\.

### HostingDeployment resources<a name="migrate-resources-to-new-operators-hosting-deployment"></a>

The HostingDeployment resource consists of multiple SageMaker resources: `Endpoint`, `EndpointConfig`, and each `Model`\. If you adopt a SageMaker endpoint in ACK, you need to adopt the `Endpoint`, `EndpointConfig`, and each `Model` separately\. The `Endpoint`, `EndpointConfig`, and `Model` names can be found in status of the HostingDeployment resource \(`status.endpointName`, `status.endpointConfigName`, and `status.modelNames`\)\.

For a list of all supported SageMaker resources, refer to the [ACK API Reference](https://aws-controllers-k8s.github.io/community/reference/)\.

## Clean up old resources<a name="migrate-resources-to-new-operators-cleanup"></a>

After the new SageMaker Operators for Kubernetes adopt your resources, you can uninstall old operators and clean up old resources\.

### Step 1: Uninstall the old operator<a name="migrate-resources-to-new-operators-uninstall"></a>

To uninstall the old operator, see [Delete operators](kubernetes-sagemaker-operators-end-of-support.md#delete-operators)\.

**Warning**  
Uninstall the old operator before deleting any old resources\.

### Step 2: Remove finalizers and delete old resources<a name="migrate-resources-to-new-operators-delete-resources"></a>

**Warning**  
Before deleting old resources, be sure that you have uninstalled the old operator\.

After uninstalling the old operator, you must explicitly remove the finalizers to delete old operator resources\. The following sample script shows how to delete all training jobs managed by the old operator in a given namespace\. You can use a similar pattern to delete additional resources once they are adopted by the new operator\.

**Note**  
You must use full resource names to get resources\. For example, use `kubectl get trainingjobs.sagemaker.aws.amazon.com` instead of `kubectl get trainingjob`\.

```
namespace=sagemaker_namespace
training_jobs=$(kubectl get trainingjobs.sagemaker.aws.amazon.com -n $namespace -ojson | jq -r '.items | .[] | .metadata.name')
 
for job in $training_jobs
do
    echo "Deleting $job resource in $namespace namespace"
    kubectl patch trainingjobs.sagemaker.aws.amazon.com $job -n $namespace -p '{"metadata":{"finalizers":null}}' --type=merge
    kubectl delete trainingjobs.sagemaker.aws.amazon.com $job -n $namespace
done
```

## Use the new SageMaker Operators for Kubernetes<a name="migrate-resources-to-new-operators-tutorials"></a>

For in\-depth guides on using the new SageMaker Operators for Kubernetes, see [Use SageMaker Operators for Kubernetes](kubernetes-sagemaker-operators-ack.md#kubernetes-sagemaker-operators-ack-use)