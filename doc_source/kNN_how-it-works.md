# How It Works<a name="kNN_how-it-works"></a>

## Step 1: Sampling<a name="step1-k-NN-sampling"></a>

 `sample_size` determines the total number of data points to be sampled from the training data set\. For example, if the initial data set has 1,000 data points and the `sample_size` is set to “100”, where the total number of instances is 2, then each worker samples 50 data points and a total set of 100 data points is collected\. Sampling runs in linear time with respect to the number of data points\. 

## Step 2: Dimension Reduction<a name="step2-kNN-dim-reduction"></a>

The current implementation of k\-NN provides two different methods of dimension reduction specified by the value of the `dimension_reduction_type` hyperparameter\. The *sign* method specifies a random projection, which uses a linear projection using a matrix of random signs, and the *fjlt* method specifies a fast Johnson\-Lindenstrauss transform, a method based on the Fourier transform\. Both methods preserve the L2 and inner product distances\. The `fjlt` method should be used when the target dimension is large and has better performance with CPU inference\. The methods differ in their computational complexity\. The `sign` method requires *O\(ndk\)* time to reduce the dimension of a batch of *n* points of dimension *d* into a target dimension *k*\. The `fjlt` method requires *O\(nd log\(d\)\)* time, but the constants involved are larger\. Note that using dimension reduction introduces noise into the data and this noise can reduce the prediction accuracy\.

## Step 3: Building an Index<a name="step3-kNN-build-index"></a>

During inference, the index is queried for the k\-nearest neighbors of a sample point and based on the labels found from the query the classification or regression prediction is made\. k\-NN provides three different types of indexes that are specified with the `index_type` parameter: a flat index, an inverted index, and an inverted index with product quantization\.

## Model Serialization<a name="kNN-model-serialization"></a>

When the k\-NN algorithm completes training, it serializes three files to prepare for inference\. 
+ model\_algo\-1: Contains the serialized index for computing the nearest neighbors\.
+ model\_algo\-1\.labels: serialized labels \(np\.float32 binary format\) to compute the predicted label based on the query result from the index\.
+ model\_algo\-1\.json: The model metadata in JSON format that stores the k and `predictor_type` hyper\-parameters from training to do inference along with other relevant state\.

With the current implementation of k\-NN, the user could successfully mutate the metadata file to change the way the predictions are computed, e\.g\. change `k` to 10 or change `predictor_type` to *regressor*\.

```
{
  "k": 5,
  "predictor_type": "classifier",
  "dimension_reduction": {"type": "sign", "seed": 3, "target_dim": 10, "input_dim": 20},
  "normalize": False,
  "version": "1.0"
}
```