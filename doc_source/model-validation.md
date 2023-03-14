# Safely validate models in production<a name="model-validation"></a>

 With SageMaker, you can test multiple models or model versions behind the same endpoint using variants\. A variant consists of an ML instance and the serving components specified in a SageMaker model\. You can have multiple variants behind an endpoint\. Each variant can have a different instance type or a SageMaker model that can be autoscaled independently of the others\. The models within the variants can be trained using different datasets, different algorithms, different ML frameworks, or any combination of all of these\. All the variants behind an endpoint share the same inference code\. SageMaker supports two types of variants, production variants and shadow variants\. 

 If you have multiple production variants behind an endpoint, then you can allocate a portion of your inference requests to each variant\. Each request is routed to only one of the production variants\. The production variant to which the request was routed provides the response to the caller\. You can compare how the production variants perform relative to each other\. 

 You can also have a shadow variant corresponding to a production variant behind an endpoint\. A portion of the inference requests that goes to the production variant is replicated to the shadow variant\. The responses of the shadow variant are logged for comparison and not returned to the caller\. This lets you test the performance of the shadow variant without exposing the caller to the response produced by the shadow variant\. 

**Topics**
+ [Production variants](model-ab-testing.md)
+ [Shadow variants](model-shadow-deployment.md)