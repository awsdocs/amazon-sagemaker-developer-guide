# How Factorization Machines Work<a name="fact-machines-howitworks"></a>

The prediction task for a factorization machine model is to estimate a function ŷ from a feature set xi to a target domain\. This domain is real\-valued for regression and binary for classification\. The factorization machine model is supervised and so has a training dataset \(xi,yj\) available\. The advantages this model presents lie in the way it uses a factorized parametrization to capture the pairwise feature interactions\. It can be represented mathematically as follows: 

![\[An image containing the equation for the factorization machine model.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/FM1.jpg)

 The three terms in this equation correspond respectively to the three components of the model: 
+ The w0 term represents the global bias\.
+ The wi linear terms model the strength of the ith variable\.
+ The <vi,vj> factorization terms model the pairwise interaction between the ith and jth variable\.

The global bias and linear terms are the same as in a linear model\. The pairwise feature interactions are modeled in the third term as the inner product of the corresponding factors learned for each feature\. Learned factors can also be considered as embedding vectors for each feature\. For example, in a classification task, if a pair of features tends to co\-occur more often in positive labeled samples, then the inner product of their factors would be large\. In other words, their embedding vectors would be close to each other in cosine similarity\. For more information about the factorization machine model, see [Factorization Machines](https://www.csie.ntu.edu.tw/~b97053/paper/Rendle2010FM.pdf)\.

For regression tasks, the model is trained by minimizing the squared error between the model prediction ŷn and the target value yn\. This is known as the square loss:

![\[An image containing the equation for square loss.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/FM2.jpg)

For a classification task, the model is trained by minimizing the cross entropy loss, also known as the log loss: 

![\[An image containing the equation for log loss.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/FM3.jpg)

where: 

![\[An image containing the logistic function of the predicted values.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/FM4.jpg)

For more information about loss functions for classification, see [Loss functions for classification](https://en.wikipedia.org/wiki/Loss_functions_for_classification)\.