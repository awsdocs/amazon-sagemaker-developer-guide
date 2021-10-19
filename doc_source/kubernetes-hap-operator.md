# HostingAutoscalingPolicy \(HAP\) Operator<a name="kubernetes-hap-operator"></a>

The HostingAutoscalingPolicy \(HAP\) operator takes a list of resource IDs as input and applies the same policy to each of them\. Each resource ID is a combination of an endpoint name and a variant name\. The HAP operator performs two steps: it registers the resource IDs and then applies the scaling policy to each resource ID\. `Delete` undoes both actions\. You can apply the HAP to an existing SageMaker endpoint or you can create a new SageMaker endpoint using the [HostingDeployment operator](https://docs.aws.amazon.com/sagemaker/latest/dg/hosting-deployment-operator.html#create-a-hostingdeployment)\. You can read more about SageMaker autoscaling in the [ Application Autoscaling Policy documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling.html)\.

**Note**  
In your `kubectl` commands, you can use the short form, `hap`, in place of `hostingautoscalingpolicy`\.

**Topics**
+ [Create a HostingAutoscalingPolicy Using a YAML File](#kubernetes-hap-job-yaml)
+ [List HostingAutoscalingPolicies](#kubernetes-hap-list)
+ [Describe a HostingAutoscalingPolicy](#kubernetes-hap-describe)
+ [Update a HostingAutoscalingPolicy](#kubernetes-hap-update)
+ [Delete a HostingAutoscalingPolicy](#kubernetes-hap-delete)
+ [Update or Delete an Endpoint with a HostingAutoscalingPolicy](#kubernetes-hap-update-delete-endpoint)

## Create a HostingAutoscalingPolicy Using a YAML File<a name="kubernetes-hap-job-yaml"></a>

Use a YAML file to create a HostingAutoscalingPolicy \(HAP\) that applies a predefined or custom metric to one or multiple SageMaker endpoints\.

Amazon SageMaker requires specific values in order to apply autoscaling to your variant\. If these values are not specified in the YAML spec, the HAP operator applies the following default values\.

```
# Do not change
Namespace                    = "sagemaker"
# Do not change
ScalableDimension            = "sagemaker:variant:DesiredInstanceCount"
# Only one supported
PolicyType                   = "TargetTrackingScaling"
# This is the default policy name but can be changed to apply a custom policy
DefaultAutoscalingPolicyName = "SageMakerEndpointInvocationScalingPolicy"
```

Use the following samples to create a HAP that applies a predefined or custom metric to one or multiple endpoints\.

### Sample 1: Apply a Predefined Metric to a Single Endpoint Variant<a name="kubernetes-hap-predefined-metric"></a>

1. Download the sample YAML file for a predefined metric using the following command:

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/hap-predefined-metric.yaml
   ```

1. Edit the YAML file to specify your `endpointName`, `variantName`, and `region`\.

1. Use one of the following commands to apply a predefined metric to a single resource ID \(endpoint name and variant name combination\)\.

   For cluster\-scoped installation:

   ```
   kubectl apply -f hap-predefined-metric.yaml
   ```

   For namespace\-scoped installation:

   ```
   kubectl apply -f hap-predefined-metric.yaml -n <NAMESPACE>
   ```

### Sample 2: Apply a Custom Metric to a Single Endpoint Variant<a name="kubernetes-hap-custom-metric"></a>

1. Download the sample YAML file for a custom metric using the following command:

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/hap-custom-metric.yaml
   ```

1. Edit the YAML file to specify your `endpointName`, `variantName`, and `region`\.

1. Use one of the following commands to apply a custom metric to a single resource ID \(endpoint name and variant name combination\) in place of the recommended `SageMakerVariantInvocationsPerInstance`\.
**Note**  
Amazon SageMaker does not check the validity of your YAML spec\.

   For cluster\-scoped installation:

   ```
   kubectl apply -f hap-custom-metric.yaml
   ```

   For namespace\-scoped installation:

   ```
   kubectl apply -f hap-custom-metric.yaml -n <NAMESPACE>
   ```

### Sample 3: Apply a Scaling Policy to Multiple Endpoints and Variants<a name="kubernetes-hap-scaling-policy"></a>

You can use the HAP operator to apply the same scaling policy to multiple resource IDs\. A separate `scaling_policy` request is created for each resource ID \(endpoint name and variant name combination\)\.

1. Download the sample YAML file for a predefined metric using the following command:

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/hap-predefined-metric.yaml
   ```

1. Edit the YAML file to specify your `region` and multiple `endpointName` and `variantName` values\.

1. Use one of the following commands to apply a predefined metric to multiple resource IDs \(endpoint name and variant name combinations\)\.

   For cluster\-scoped installation:

   ```
   kubectl apply -f hap-predefined-metric.yaml
   ```

   For namespace\-scoped installation:

   ```
   kubectl apply -f hap-predefined-metric.yaml -n <NAMESPACE>
   ```

### Considerations for HostingAutoscalingPolicies for Multiple Endpoints and Variants<a name="kubernetes-hap-scaling-considerations"></a>

The following considerations apply when you use multiple resource IDs:
+ If you apply a single policy across multiple resource IDs, one PolicyARN is created per resource ID\. Five endpoints have five PolicyARNs\. When you run the `describe` command on the policy, the responses show up as one job and include a single job status\.
+ If you apply a custom metric to multiple resource IDs, the same dimension or value is used for all the resource ID \(variant\) values\. For example, if you apply a customer metric for instances 1\-5, and the endpoint variant dimension is mapped to variant 1, when variant 1 exceeds the metrics, all endpoints are scaled up or down\.
+ The HAP operator supports updating the list of resource IDs\. If you modify, add, or delete resource IDs to the spec, the autoscaling policy is removed from the previous list of variants and applied to the newly specified resource ID combinations\. Use the [https://docs.aws.amazon.com/sagemaker/latest/dg/kubernetes-hap-operator.html#kubernetes-hap-describe](https://docs.aws.amazon.com/sagemaker/latest/dg/kubernetes-hap-operator.html#kubernetes-hap-describe) command to list the resource IDs to which the policy is currently applied\.

## List HostingAutoscalingPolicies<a name="kubernetes-hap-list"></a>

Use one of the following commands to list all HostingAutoscalingPolicies \(HAPs\) created using the HAP operator\.

For cluster\-scoped installation:

```
kubectl get hap
```

For namespace\-scoped installation:

```
kubectl get hap -n <NAMESPACE>
```

Your output should look similar to the following:

```
NAME             STATUS   CREATION-TIME
hap-predefined   Created  2021-07-13T21:32:21Z
```

Use the following command to check the status of your HostingAutoscalingPolicy \(HAP\)\.

```
kubectl get hap <job-name>
```

One of the following values is returned:
+ `Reconciling` – Certain types of errors show the status as `Reconciling` instead of `Error`\. Some examples are server\-side errors and endpoints in the `Creating` or `Updating` state\. Check the `Additional` field in status or operator logs for more details\.
+ `Created`
+ `Error`

**To view the autoscaling endpoint to which you applied the policy**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left side panel, expand **Inference**\.

1. Choose **Endpoints**\.

1. Select the name of the endpoint of interest\.

1. Scroll to the **Endpoint runtime settings** section\.

## Describe a HostingAutoscalingPolicy<a name="kubernetes-hap-describe"></a>

Use the following command to get more details about a HostingAutoscalingPolicy \(HAP\)\. These commands are typically used for debugging a problem or checking the resource IDs \(endpoint name and variant name combinations\) of a HAP\.

```
kubectl describe hap <job-name>
```

## Update a HostingAutoscalingPolicy<a name="kubernetes-hap-update"></a>

The HostingAutoscalingPolicy \(HAP\) operator supports updates\. You can edit your YAML spec to change the values and then reapply the policy\. The HAP operator deletes the existing policy and applies the new policy\.

## Delete a HostingAutoscalingPolicy<a name="kubernetes-hap-delete"></a>

Use one of the following commands to delete a HostingAutoscalingPolicy \(HAP\) policy\.

For cluster\-scoped installation:

```
kubectl delete hap hap-predefined
```

For namespace\-scoped installation:

```
kubectl delete hap hap-predefined -n <NAMESPACE>
```

This command deletes the scaling policy and deregisters the scaling target from Kubernetes\. This command returns the following output:

```
hostingautoscalingpolicies.sagemaker.aws.amazon.com "hap-predefined" deleted
```

## Update or Delete an Endpoint with a HostingAutoscalingPolicy<a name="kubernetes-hap-update-delete-endpoint"></a>

To update an endpoint that has a HostingAutoscalingPolicy \(HAP\), use the `kubectl` `delete` command to remove the HAP, update the endpoint, and then reapply the HAP\.

To delete an endpoint that has a HAP, use the `kubectl` `delete` command to remove the HAP before you delete the endpoint\.