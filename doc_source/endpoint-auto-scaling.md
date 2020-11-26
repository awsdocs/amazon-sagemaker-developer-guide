# Automatically Scale Amazon SageMaker Models<a name="endpoint-auto-scaling"></a>

Amazon SageMaker supports automatic scaling \(autoscaling\) for your hosted models\. *Autoscaling* dynamically adjusts the number of instances provisioned for a model in response to changes in your workload\. When the workload increases, autoscaling brings more instances online\. When the workload decreases, autoscaling removes unnecessary instances so that you don't pay for provisioned instances that you aren't using\.

**Topics**
+ [Prerequisites](endpoint-auto-scaling-prerequisites.md)
+ [Configure model autoscaling with the console](endpoint-auto-scaling-add-console.md)
+ [Register a model](endpoint-auto-scaling-add-policy.md)
+ [Define a scaling policy](endpoint-auto-scaling-add-code-define.md)
+ [Apply a scaling policy](endpoint-auto-scaling-add-code-apply.md)
+ [Edit a scaling policy](endpoint-auto-scaling-edit.md)
+ [Delete a scaling policy](endpoint-auto-scaling-delete.md)
+ [Query Endpoint Autoscaling History](endpoint-scaling-query-history.md)
+ [Update or delete endpoints that use automatic scaling](endpoint-scaling.md)
+ [Load testing your autoscaling configuration](endpoint-scaling-loadtest.md)
+ [Use AWS CloudFormation to update autoscaling policies](endpoint-scaling-cloudformation.md)