# K\-Nearest Neighbors Hyperparameters<a name="kNN_hyperparameters"></a>


| Parameter Name | Description | 
| --- | --- | 
| feature\_dim |  Number of features in the input data\. Required\. Valid values: positive integer\. Default value: \-  | 
| mini\_batch\_size |  The number of observations per mini\-batch for the data iterator\. Optional\. Valid values: positive integer Default value: 5000  | 
| k |  Number of nearest neighbors\. Required\. Valid values: positive integer Default value: \-  | 
| predictor\_type |  Type of inference to use on the data labels\. Required\.  Valid values: *classifier* for classification or *regressor* for regression\. Default value: \-  | 
| sample\_size |  Number of data points to be sampled from the training data set\. Required\.  Valid values: positive integer Default value: \-  | 
| dimension\_reduction\_type |  Dimension reduction method\.  Valid values: *sign* for random projection or *fjlt* for the fast Johnson\-Lindenstrauss Transform\. Default value: No dimension reduction  | 
| dimension\_reduction\_target |  Target dimension to reduce to\. Required when `dimension_reduction_type` is specified\. Valid values: Number between 0 and feature\_dim, exclusive\. Default value: \-  | 
| index\_type |  Type of index\. Valid values: *faiss\.Flat*, *faiss\.IVFFlat*, *faiss\.IVFPQ*\. Default values: *faiss\.Flat*  | 
| index\_metric |  Metric to measure distance between points when finding nearest neighbors\. When training with `index_type` = `faiss.IVFPQ`, `index_metric` of `INNER_PRODUCT` or `COSINE` is not supported\. Valid values: *L2* for Euclidean\-distance, *INNER\_PRODUCT* for inner\-product distance, *COSINE* for cosine similarity\. Default value: *L2*  | 
| faiss\_index\_ivf\_nlists |  Number of centroids to construct in the index when `index_type` is *faiss\.IVFFlat* or *faiss\.IVFPQ*\. Valid values: positive integer Default value: *auto*, which resolves to `sqrt(sample_size)`\.  | 
| faiss\_index\_pq\_m | Number of vector sub\-components to construct in the index when `index_type` is set to *faiss\.IVFPQ*\. FAISS requires that the value of `faiss_index_pq_m` is a divisor of the data dimension\. As a result, if `faiss_index_pq_m` is not a divisor of the data dimension, we internally increase the data dimension to smallest integer divisible by `faiss_index_pq_m`\. If there is no dimension reduction applied we add a padding of zeros\. If there is dimension reduction applied, we increase the value of the `dimension_reduction_target` hyper\-parameter\. Valid values: One of the following positive integers: 1, 2, 3, 4, 8, 12, 16, 20, 24, 28, 32, 40, 48, 56, 64, 96 Default value: \- | 