# K\-Nearest Neighbors<a name="k-nearest-neighbors"></a>

Amazon SageMaker k\-nearest neighbors \(k\-NN\) is an index\-based algorithm\. The algorithm employs a non\-parametric method that can be used for classification or regression\. The `predictor_type` hyperparameter specifies the type of inference as `classifier` or `regressor`\. For classification, the k points closest to the sample point are queried and the most frequent of their class labels is returned as the predicted label\. For regression, the k closest points to the sample point are queried and the average of their labels is returned as the predicted value\. 

The k\-NN algorithm training consists of three steps: sampling, dimension reduction, and building an index\. Sampling reduces the initial data set in size to fit into memory\. In the dimension reduction step, the feature dimension of the data is lowered to reduce the memory footprint of the k\-NN model and to lower the inference latency\. Two types of dimension reduction methods are provided: random projection and the fast Johnson\-Lindenstrauss transform\. Typically, dimension reduction is used for high dimensional \(d > 1000\) datasets to avoid the “curse of dimensionality” that troubles the statistical analysis of data that becomes sparse as dimensionality increases\. The main objective of k\-NN's training is to construct the index\. The index constructed in the final stage enables efficient lookups of distances between points whose labels are to be determined and the k nearest points to use for the inference\.

## Input/Output Interface<a name="kNN-input_output"></a>

Amazon SageMaker k\-NN supports train and test data channels\.
+ A *train channel* for data to be sampled and constructed into the k\-NN index\.
+ A *test channel* that emits scores in logs, one line per mini\-batch: accuracy for `classifier`, mean squared error \(mse\) for `regressor` for score\.

For training inputs, k\-NN supports `text/csv` and `application/x-recordio-protobuf` data formats\. For input type `text/csv`, the first `label_size` columns are interpreted as the label vector for that row\. k\-NN can be trained in File or Pipe mode when using the `application/x-recordio-protobuf` format, but only in File mode for the `text/csv` format\.

For inference inputs, k\-NN supports the `application/json`, `application/x-recordio-protobuf`, and `text/csv` data formats\. The `text/csv` format accepts a `label_size` and encoding parameter\. It assumes a `label_size` of 0 and a utf\-8 encoding\.

For inference outputs, k\-NN supports the `application/json` and `application/x-recordio-protobuf` data formats\. These two data formats also support a verbose output mode in which the API provides the search results with the distances vector sorted from smallest to largest, with corresponding elements in the labels vector\.

For batch transform, k\-NN supports the `application/jsonlines` data format for both input and output\. An example input is as follows:

```
content-type: application/jsonlines

{"features": [1.5, 16.0, 14.0, 23.0]}
{"data": {"features": {"values": [1.5, 16.0, 14.0, 23.0]}}
```

An example output is as follows:

```
accept: application/jsonlines

{"predicted_label": 0.0}
{"predicted_label": 2.0}
```

## EC2 Instance Recommendation<a name="kNN-instances"></a>

### Training<a name="kNN-instances-training"></a>

To start with, try a CPU machine such as ml\.m5\.2xlarge or a GPU machine such as ml\.p2\.xlarge\.

### Inference<a name="kNN-instances-inference"></a>

Inference requests from CPU machines are generally expected to have a lower average latency than requests from GPU due to the CPU to GPU communication tax when using GPU hardware\. However, GPU machines are generally expected to have a higher throughput for larger batches\.