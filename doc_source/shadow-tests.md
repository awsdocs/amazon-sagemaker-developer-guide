# Shadow tests<a name="shadow-tests"></a>

 With Amazon SageMaker you can evaluate any changes to your model serving infrastructure by comparing its performance against the currently deployed infrastructure\. This practice is known as shadow testing\. Shadow testing can help you catch potential configuration errors and performance issues before they impact end users\. With SageMaker, you don't need to invest in building your shadow testing infrastructure, so you can focus on model development\. 

 You can use this capability to validate changes to any component of your production variant, namely the model, the container, or the instance, without any end user impact\. It is useful in situations including but not limited to the following: 
+  You are considering promoting a new model that has been validated offline to production, but want to evaluate operational performance metrics such as latency and error rate before making this decision\. 
+  You are considering changes to your serving infrastructure container, such as patching vulnerabilities or upgrading to newer versions, and want to assess the impact of these changes prior to promotion to production\. 
+  You are considering changing your ML instance and want to evaluate how the new instance would perform with live inference requests\. 

 The SageMaker console provides a guided experience to manage the workflow of shadow testing\. You can setup shadow tests for a predefined duration of time, monitor the progress of the test through a live dashboard, clean up upon completion, and act on the results\. Select a production variant you want to test against, and SageMaker automatically deploys the new variant in shadow mode and routes a copy of the inference requests to it in real time within the same endpoint\. Only the responses of the production variant are returned to the calling application\. You can choose to discard or log the responses of the shadow variant for offline comparison\. For more information on production and shadow variants, see [Safely validate models in production](model-validation.md)\. 

 See [Create a shadow test](shadow-tests-create.md) for instructions on creating a shadow test\. 