# Exclusions<a name="deployment-guardrails-exclusions"></a>

You can only use deployment guardrails with newly created endpoints\. There are also feature\-based exclusions that make your endpoint incompatible with deployment guardrails at this time\. If your endpoint uses any of the following features, you cannot use deployment guardrails on your endpoint, and your endpoint will fall back to using a blue/green deployment with all at once traffic shifting and no final baking period\.
+ Marketplace containers
+ Multi\-container endpoints
+ Multi\-model endpoints
+ Network isolation
+ Multi\-variant endpoints
+ Endpoints that use Inf1 \(Inferentia\-based\) instances
+ AWS KMS\-enabled endpoints
+ Endpoints that use Amazon SageMaker Model Monitor \(with data capture enabled\)
+ Amazon Elastic Inference endpoints