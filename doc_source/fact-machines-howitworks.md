# How Factorization Machines Work<a name="fact-machines-howitworks"></a>

The factorization machine model with pairwise feature interactions can be written as follows: 

![\[TBD\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/FM1.jpg)

For regression tasks, the model is trained by minimizing the squared error between model prediction and the target value \(in other words, square loss\):

![\[TBD\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/FM2.jpg)

For a classification task, the model is trained by minimizing the log loss: 

![\[TBD\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/FM3.jpg)

where: 

![\[TBD\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/FM4.jpg)

The model has three components\. 
+ Bias terms
+ Linear terms
+ Factorization \(interaction\) terms

Bias and linear terms are the same as in a linear model\. Pairwise feature interactions are modeled as the inner product of the corresponding factors learned for each feature\. Learned factors can also be considered as embedding vectors for each feature\. For example, in a classification task, if a pair of features tends to co\-occur more often in positive labeled samples, then the inner product of their factors would be high\. In other words, their embedding vectors would be close to each other in cosine similarity\. 