# How PCA Works<a name="how-pca-works"></a>

Principal Component Analysis \(PCA\) is a learning algorithm that reduces the dimensionality \(number of features\) within a dataset while still retaining as much information as possible\. 

PCA reduces dimensionality by finding a new set of features called *components*, which are composites of the original features, but are uncorrelated with one another\. The first component accounts for the largest possible variability in the data, the second component the second most variability, and so on\.

It is an unsupervised dimensionality reduction algorithm\. In unsupervised learning, labels that might be associated with the objects in the training dataset aren't used\.

Given the input of a matrix with rows ![\[x_1,…,x_n\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-39b.png) each of dimension `1 * d`, the data is partitioned into mini\-batches of rows and distributed among the training nodes \(workers\)\. Each worker then computes a summary of its data\. The summaries of the different workers are then unified into a single solution at the end of the computation\. 

**Modes**

The Amazon SageMaker PCA algorithm uses either of two modes to calculate these summaries, depending on the situation:
+ **regular**: for datasets with sparse data and a moderate number of observations and features\.
+ **randomized**: for datasets with both a large number of observations and features\. This mode uses an approximation algorithm\. 

As the algorithm's last step, it performs the singular value decomposition on the unified solution, from which the principal components are then derived\.

## Mode 1: Regular<a name="mode-1"></a>

The workers jointly compute both ![\[\sum x_i^T x_i\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-1b.png) and ![\[\sum x_i\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-2b.png) \.

**Note**  
Because ![\[x_i\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-3b.png) are `1 * d` row vectors, ![\[x_i^T x_i\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-4b.png) is a matrix \(not a scalar\)\. Using row vectors within the code allows us to obtain efficient caching\.

The covariance matrix is computed as ![\[\sum x_i^T x_i - (1/n) (\sum x_i)^T \sum x_i\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-32b.png) , and its top `num_components` singular vectors form the model\.

**Note**  
If `subtract_mean` is `False`, we avoid computing and subtracting ![\[\sum x_i\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-2b.png) \.

Use this algorithm when the dimension `d` of the vectors is small enough so that ![\[d^2\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-7b.png) can fit in memory\.

## Mode 2: Randomized<a name="mode-2"></a>

When the number of features in the input dataset is large, we use a method to approximate the covariance metric\. For every mini\-batch ![\[X_t\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-23b.png) of dimension `b * d`, we randomly initialize a `(num_components + extra_components) * b` matrix that we multiply by each mini\-batch, to create a `(num_components + extra_components) * d` matrix\. The sum of these matrices is computed by the workers, and the servers perform SVD on the final `(num_components + extra_components) * d` matrix\. The top right `num_components` singular vectors of it are the approximation of the top singular vectors of the input matrix\.

Let ![\[\ell\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-38b.png) ` = num_components + extra_components`\. Given a mini\-batch ![\[X_t\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-23b.png) of dimension `b * d`, the worker draws a random matrix ![\[H_t\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-24b.png) of dimension ![\[\ell * b\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-38.png) \. Depending on whether the environment uses a GPU or CPU and the dimension size, the matrix is either a random sign matrix where each entry is `+-1` or a *FJLT* \(fast Johnson Lindenstrauss transform; for information, see [FJLT Transforms](https://www.cs.princeton.edu/~chazelle/pubs/FJLT-sicomp09.pdf) and the follow\-up papers\)\. The worker then computes ![\[H_t X_t\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-26b.png) and maintains ![\[B = \sum H_t X_t\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-27b.png) \. The worker also maintains ![\[h^T\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-28b.png) , the sum of columns of ![\[H_1,..,H_T\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-29b.png) \(`T` being the total number of mini\-batches\), and `s`, the sum of all input rows\. After processing the entire shard of data, the worker sends the server `B`, `h`, `s`, and `n` \(the number of input rows\)\.

Denote the different inputs to the server as ![\[B^1, h^1, s^1, n^1,…\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-30b.png) The server computes `B`, `h`, `s`, `n` the sums of the respective inputs\. It then computes ![\[C = B – (1/n) h^T s\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/PCA-31b.png) , and finds its singular value decomposition\. The top\-right singular vectors and singular values of `C` are used as the approximate solution to the problem\.