# Exclusions<a name="deployment-guardrails-exclusions"></a>

You can only use deployment guardrails with newly created endpoints\. There are also feature\-based exclusions that make your endpoint incompatible with deployment guardrails at this time\. If your endpoint uses any of the following features, you cannot use deployment guardrails on your endpoint, and your endpoint will fall back to using a blue/green deployment with all at once traffic shifting and no final baking period\.
+ Marketplace containers
+ Endpoints that use Inf1 \(Inferentia\-based\) instances
+ Amazon Elastic Inference endpoints