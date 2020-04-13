# How the k\-NN Algorithm Works<a name="kNN_how-it-works"></a>

## Step 1: Sample<a name="step1-k-NN-sampling"></a>

To specify the total number of data points to be sampled from the training dataset, use the `sample_size`parameter\. For example, if the initial dataset has 1,000 data points and the `sample_size` is set to 100, where the total number of instances is 2, each worker would sample 50 points\. A total set of 100 data points would be collected\. Sampling runs in linear time with respect to the number of data points\. 

## Step 2: Perform Dimension Reduction<a name="step2-kNN-dim-reduction"></a>

The current implementation of the k\-NN algorithm has two methods of dimension reduction\. You specify the method in the `dimension_reduction_type` hyperparameter\. The `sign` method specifies a random projection, which uses a linear projection using a matrix of random signs, and the `fjlt` method specifies a fast Johnson\-Lindenstrauss transform, a method based on the Fourier transform\. Both methods preserve the L2 and inner product distances\. The `fjlt` method should be used when the target dimension is large and has better performance with CPU inference\. The methods differ in their computational complexity\. The `sign` method requires O\(ndk\) time to reduce the dimension of a batch of n points of dimension d into a target dimension k\. The `fjlt` method requires O\(nd log\(d\)\) time, but the constants involved are larger\. Using dimension reduction introduces noise into the data and this noise can reduce prediction accuracy\.

## Step 3: Build an Index<a name="step3-kNN-build-index"></a>

During inference, the algorithm queries the index for the k\-nearest\-neighbors of a sample point\. Based on the references to the points, the algorithm makes the classification or regression prediction\. It makes the prediction based on the class labels or values provided\. k\-NN provides three different types of indexes: a flat index, an inverted index, and an inverted index with product quantization\. You specify the type with the `index_type` parameter\.

## Serialize the Model<a name="kNN-model-serialization"></a>

When the k\-NN algorithm finishes training, it serializes three files to prepare for inference\. 
+ model\_algo\-1: Contains the serialized index for computing the nearest neighbors\.
+ model\_algo\-1\.labels: Contains serialized labels \(np\.float32 binary format\) for computing the predicted label based on the query result from the index\.
+ model\_algo\-1\.json: Contains the JSON\-formatted model metadata which stores the `k` and `predictor_type` hyper\-parameters from training for inference along with other relevant state\.

With the current implementation of k\-NN, you can modify the metadata file to change the way predictions are computed\. For example, you can change `k` to 10 or change `predictor_type` to *regressor*\.

```
{
  "k": 5,
  "predictor_type": "classifier",
  "dimension_reduction": {"type": "sign", "seed": 3, "target_dim": 10, "input_dim": 20},
  "normalize": False,
  "version": "1.0"
}
```