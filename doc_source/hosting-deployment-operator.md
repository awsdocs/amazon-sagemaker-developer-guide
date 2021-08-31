# HostingDeployment operator<a name="hosting-deployment-operator"></a>

HostingDeployment operators support creating and deleting an endpoint, as well as updating an existing endpoint, for real\-time inference\. The hosting deployment operator reconciles your specified hosting deployment job spec to SageMaker by creating models, endpoint\-configs and endpoints in SageMaker\. You can learn more about SageMaker inference in the SageMaker [CreateEndpoint API documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html)\. 

**Topics**
+ [Configure a HostingDeployment Resource](#configure-a-hostingdeployment-resource)
+ [Create a HostingDeployment](#create-a-hostingdeployment)
+ [List HostingDeployments](#list-hostingdeployments)
+ [Describe a HostingDeployment](#describe-a-hostingdeployment)
+ [Invoking the Endpoint](#invoking-the-endpoint)
+ [Update HostingDeployment](#update-hostingdeployment)
+ [Delete the HostingDeployment](#delete-the-hostingdeployment)

## Configure a HostingDeployment Resource<a name="configure-a-hostingdeployment-resource"></a>

Download the sample YAML file for the hosting deployment job using the following command: 

```
wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/xgboost-mnist-hostingdeployment.yaml
```

The `xgboost-mnist-hostingdeployment.yaml` file has the following components that can be edited as required: 
+ *ProductionVariants*\. A production variant is a set of instances serving a single model\. SageMaker load\-balances between all production variants according to set weights\. 
+ *Models*\. A model is the containers and execution role ARN necessary to serve a model\. It requires at least a single container\. 
+ *Containers*\. A container specifies the dataset and serving image\. If you are using your own custom algorithm instead of an algorithm provided by SageMaker, the inference code must meet SageMaker requirements\. For more information, see [Using Your Own Algorithms with SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html)\. 

## Create a HostingDeployment<a name="create-a-hostingdeployment"></a>

To create a HostingDeployment, use `kubectl` to apply the file `hosting.yaml` with the following command: 

```
kubectl apply -f hosting.yaml
```

SageMaker creates an endpoint with the specified configuration\. You incur charges for SageMaker resources used during the lifetime of your endpoint\. You do not incur any charges once your endpoint is deleted\. 

The creation process takes approximately 10 minutes\. 

## List HostingDeployments<a name="list-hostingdeployments"></a>

To verify that the HostingDeployment was created, use the following command: 

```
kubectl get hostingdeployments
```

Your output should look like the following: 

```
NAME           STATUS     SAGEMAKER-ENDPOINT-NAME
host-xgboost   Creating   host-xgboost-def0e83e0d5f11eaaa450aSMLOGS
```

### HostingDeployment Status Values<a name="hostingdeployment-status-values"></a>

The status field can be one of several values: 
+ `SynchronizingK8sJobWithSageMaker`: The operator is preparing to create the endpoint\. 
+ `ReconcilingEndpoint`: The operator is creating, updating, or deleting endpoint resources\. If the HostingDeployment remains in this state, use `kubectl describe` to see the reason in the `Additional` field\. 
+ `OutOfService`: Endpoint is not available to take incoming requests\. 
+ `Creating`: [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html) is executing\. 
+ `Updating`: [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html) or [UpdateEndpointWeightsAndCapacities](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpointWeightsAndCapacities.html) is executing\. 
+ `SystemUpdating`: Endpoint is undergoing maintenance and cannot be updated or deleted or re\-scaled until it has completed\. This maintenance operation does not change any customer\-specified values such as VPC config, KMS encryption, model, instance type, or instance count\. 
+ `RollingBack`: Endpoint fails to scale up or down or change its variant weight and is in the process of rolling back to its previous configuration\. Once the rollback completes, endpoint returns to an `InService` status\. This transitional status only applies to an endpoint that has autoscaling enabled and is undergoing variant weight or capacity changes as part of an [UpdateEndpointWeightsAndCapacities](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpointWeightsAndCapacities.html) call or when the [UpdateEndpointWeightsAndCapacities](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpointWeightsAndCapacities.html) operation is called explicitly\. 
+ `InService`: Endpoint is available to process incoming requests\. 
+ `Deleting`: [DeleteEndpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DeleteEndpoint.html) is executing\. 
+ `Failed`: Endpoint could not be created, updated, or re\-scaled\. Use [DescribeEndpoint:FailureReason](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeEndpoint.html#SageMaker-DescribeEndpoint-response-FailureReason) for information about the failure\. [DeleteEndpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DeleteEndpoint.html) is the only operation that can be performed on a failed endpoint\. 

## Describe a HostingDeployment<a name="describe-a-hostingdeployment"></a>

You can obtain debugging details using the `describe` `kubectl` command\.

```
kubectl describe hostingdeployment
```

Your output should look like the following: 

```
Name:         host-xgboost
Namespace:    default
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"sagemaker.aws.amazon.com/v1","kind":"HostingDeployment","metadata":{"annotations":{},"name":"host-xgboost","namespace":"def..."
API Version:  sagemaker.aws.amazon.com/v1
Kind:         HostingDeployment
Metadata:
  Creation Timestamp:  2019-11-22T19:40:00Z
  Finalizers:
    sagemaker-operator-finalizer
  Generation:        1
  Resource Version:  4258134
  Self Link:         /apis/sagemaker.aws.amazon.com/v1/namespaces/default/hostingdeployments/host-xgboost
  UID:               def0e83e-0d5f-11ea-aa45-0a3507uiduid
Spec:
  Containers:
    Container Hostname:  xgboost
    Image:               123456789012.dkr.ecr.us-east-2.amazonaws.com/xgboost:latest
    Model Data URL:      s3://my-bucket/inference/xgboost-mnist/model.tar.gz
  Models:
    Containers:
      xgboost
    Execution Role Arn:  arn:aws:iam::123456789012:role/service-role/AmazonSageMaker-ExecutionRole
    Name:                xgboost-model
    Primary Container:   xgboost
  Production Variants:
    Initial Instance Count:  1
    Instance Type:           ml.c5.large
    Model Name:              xgboost-model
    Variant Name:            all-traffic
  Region:                    us-east-2
Status:
  Creation Time:         2019-11-22T19:40:04Z
  Endpoint Arn:          arn:aws:sagemaker:us-east-2:123456789012:endpoint/host-xgboost-def0e83e0d5f11eaaaexample
  Endpoint Config Name:  host-xgboost-1-def0e83e0d5f11e-e08f6c510d5f11eaaa450aexample
  Endpoint Name:         host-xgboost-def0e83e0d5f11eaaa450a350733ba06
  Endpoint Status:       Creating
  Endpoint URL:          https://runtime.sagemaker.us-east-2.amazonaws.com/endpoints/host-xgboost-def0e83e0d5f11eaaaexample/invocations
  Last Check Time:       2019-11-22T19:43:57Z
  Last Modified Time:    2019-11-22T19:40:04Z
  Model Names:
    Name:   xgboost-model
    Value:  xgboost-model-1-def0e83e0d5f11-df5cc9fd0d5f11eaaa450aexample
Events:     <none>
```

The status field provides more information using the following fields: 
+ `Additional`: Additional information about the status of the hosting deployment\. This field is optional and only gets populated in case of error\. 
+ `Creation Time`: When the endpoint was created in SageMaker\. 
+ `Endpoint ARN`: The SageMaker endpoint ARN\. 
+ `Endpoint Config Name`: The SageMaker name of the endpoint configuration\. 
+ `Endpoint Name`: The SageMaker name of the endpoint\. 
+ `Endpoint Status`: The status of the endpoint\. 
+ `Endpoint URL`: The HTTPS URL that can be used to access the endpoint\. For more information, see [Deploy a Model on SageMaker Hosting Services](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-hosting.html)\. 
+ `FailureReason`: If a create, update, or delete command fails, the cause is shown here\. 
+ `Last Check Time`: The last time the operator checked the status of the endpoint\. 
+ `Last Modified Time`: The last time the endpoint was modified\. 
+ `Model Names`: A key\-value pair of HostingDeployment model names to SageMaker model names\. 

## Invoking the Endpoint<a name="invoking-the-endpoint"></a>

Once the endpoint status is `InService`, you can invoke the endpoint in two ways: using the AWS CLI, which does authentication and URL request signing, or using an HTTP client like cURL\. If you use your own client, you need to do AWS v4 URL signing and authentication on your own\. 

To invoke the endpoint using the AWS CLI, run the following command\. Make sure to replace the Region and endpoint\-name with your endpoint's Region and SageMaker endpoint name\. This information can be obtained from the output of `kubectl describe`\. 

```
# Invoke the endpoint with mock input data.
aws sagemaker-runtime invoke-endpoint \
  --region us-east-2 \
  --endpoint-name <endpoint name> \
  --body $(seq 784 | xargs echo | sed 's/ /,/g') \
  >(cat) \
  --content-type text/csv > /dev/null
```

For example, if your Region is `us-east-2` and your endpoint config name is `host-xgboost-f56b6b280d7511ea824b129926example`, then the following command would invoke the endpoint: 

```
aws sagemaker-runtime invoke-endpoint \
  --region us-east-2 \
  --endpoint-name host-xgboost-f56b6b280d7511ea824b1299example \
  --body $(seq 784 | xargs echo | sed 's/ /,/g') \
  >(cat) \
  --content-type text/csv > /dev/null
4.95847082138
```

Here, `4.95847082138` is the prediction from the model for the mock data\. 

## Update HostingDeployment<a name="update-hostingdeployment"></a>

1. Once a HostingDeployment has a status of `InService`, it can be updated\. It might take about 10 minutes for HostingDeployment to be in service\. To verify that the status is `InService`, use the following command: 

   ```
   kubectl get hostingdeployments
   ```

1. The HostingDeployment can be updated before the status is `InService`\. The operator waits until the SageMaker endpoint is `InService` before applying the update\. 

   To apply an update, modify the `hosting.yaml` file\. For example, change the `initialInstanceCount` field from 1 to 2 as follows: 

   ```
   apiVersion: sagemaker.aws.amazon.com/v1
   kind: HostingDeployment
   metadata:
     name: host-xgboost
   spec:
       region: us-east-2
       productionVariants:
           - variantName: all-traffic
             modelName: xgboost-model
             initialInstanceCount: 2
             instanceType: ml.c5.large
       models:
           - name: xgboost-model
             executionRoleArn: arn:aws:iam::123456789012:role/service-role/AmazonSageMaker-ExecutionRole
             primaryContainer: xgboost
             containers:
               - xgboost
       containers:
           - containerHostname: xgboost
             modelDataUrl: s3://my-bucket/inference/xgboost-mnist/model.tar.gz
             image: 123456789012.dkr.ecr.us-east-2.amazonaws.com/xgboost:latest
   ```

1. Save the file, then use `kubectl` to apply your update as follows\. You should see the status change from `InService` to `ReconcilingEndpoint`, then `Updating`\. 

   ```
   $ kubectl apply -f hosting.yaml
   hostingdeployment.sagemaker.aws.amazon.com/host-xgboost configured
   
   $ kubectl get hostingdeployments
   NAME           STATUS                SAGEMAKER-ENDPOINT-NAME
   host-xgboost   ReconcilingEndpoint   host-xgboost-def0e83e0d5f11eaaa450a350abcdef
   
   $ kubectl get hostingdeployments
   NAME           STATUS     SAGEMAKER-ENDPOINT-NAME
   host-xgboost   Updating   host-xgboost-def0e83e0d5f11eaaa450a3507abcdef
   ```

SageMaker deploys a new set of instances with your models, switches traffic to use the new instances, and drains the old instances\. As soon as this process begins, the status becomes `Updating`\. After the update is complete, your endpoint becomes `InService`\. This process takes approximately 10 minutes\. 

## Delete the HostingDeployment<a name="delete-the-hostingdeployment"></a>

1. Use `kubectl` to delete a HostingDeployment with the following command: 

   ```
   kubectl delete hostingdeployments host-xgboost
   ```

   Your output should look like the following: 

   ```
   hostingdeployment.sagemaker.aws.amazon.com "host-xgboost" deleted
   ```

1. To verify that the hosting deployment has been deleted, use the following command: 

   ```
   kubectl get hostingdeployments
   No resources found.
   ```

Endpoints that have been deleted do not incur any charges for SageMaker resources\. 