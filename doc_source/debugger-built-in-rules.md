# Built\-in Rules Provided by Amazon SageMaker Debugger<a name="debugger-built-in-rules"></a>

Use the built\-in rules provided by Amazon SageMaker Debugger to analyze tensors emitted during the training of machine learning models\. These rules monitor various common conditions that are critical for the success of a training job\. There are four scopes of validity for the built\-in rules\.


**Scopes of Validity for Build\-in Rules**  

| Scope of Validity | Built\-in Rules | 
| --- | --- | 
| Deep learning frameworks \(TensorFlow, Apache MXNet, and PyTorch\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | 
| Deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) and the XGBoost algorithm  | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | 
| Deep learning applications | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | 
| XGBoost algorithm | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | 

**Topics**
+ [DeadRelu Rule](dead-relu.md)
+ [ExplodingTensor Rule](exploding-tensor.md)
+ [PoorWeightInitialization Rule](poor-weight-initialization.md)
+ [SaturatedActivation Rule](saturated-activation.md)
+ [VanishingGradient Rule](vanishing-gradient.md)
+ [WeightUpdateRatio Rule](weight-update-ratio.md)
+ [AllZero Rule](all-zero.md)
+ [ClassImbalance Rule](class-imbalance.md)
+ [Confusion Rule](confusion.md)
+ [LossNotDecreasing Rule](loss-not-decreasing.md)
+ [Overfit Rule](overfit.md)
+ [Overtraining Rule](overtraining.md)
+ [SimilarAcrossRuns Rule](similar-across-runs.md)
+ [TensorVariance Rule](tensor-variance.md)
+ [UnchangedTensor Rule](unchanged-tensor.md)
+ [CheckInputImages Rule](checkinput-mages.md)
+ [NLPSequenceRatio Rule](nlp-sequence-ratio.md)
+ [TreeDepth Rule](tree-depth.md)