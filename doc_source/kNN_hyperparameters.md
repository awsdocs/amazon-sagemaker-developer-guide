# k\-NN Hyperparameters<a name="kNN_hyperparameters"></a>


| Parameter Name | Description | 
| --- | --- | 
| feature\_dim |  The number of features in the input data\. **Required** Valid values: positive integer\.  | 
| k |  The number of nearest neighbors\. **Required** Valid values: positive integer  | 
| predictor\_type |  The type of inference to use on the data labels\. **Required** Valid values: *classifier* for classification or *regressor* for regression\.  | 
| sample\_size |  The number of data points to be sampled from the training data set\.  **Required** Valid values: positive integer  | 
| dimension\_reduction\_target |  The target dimension to reduce to\. **Required** when you specify the `dimension_reduction_type` parameter\. Valid values: positive integer greater than 0 and less than `feature_dim`\.  | 
| dimension\_reduction\_type |  The type of dimension reduction method\.  **Optional** Valid values: *sign* for random projection or *fjlt* for the fast Johnson\-Lindenstrauss transform\. Default value: No dimension reduction  | 
| faiss\_index\_ivf\_nlists |  The number of centroids to construct in the index when `index_type` is *faiss\.IVFFlat* or *faiss\.IVFPQ*\. **Optional** Valid values: positive integer Default value: *auto*, which resolves to `sqrt(sample_size)`\.  | 
| faiss\_index\_pq\_m |  The number of vector sub\-components to construct in the index when `index_type` is set to *faiss\.IVFPQ*\.  The FaceBook AI Similarity Search \(FAISS\) library requires that the value of `faiss_index_pq_m` is a divisor of the data dimension\. If `faiss_index_pq_m` is not a divisor of the data dimension, we increase the data dimension to smallest integer divisible by `faiss_index_pq_m`\. If no dimension reduction is applied, the algorithm adds a padding of zeros\. If dimension reduction is applied, the algorithm increase the value of the `dimension_reduction_target` hyper\-parameter\. **Optional** Valid values: One of the following positive integers: 1, 2, 3, 4, 8, 12, 16, 20, 24, 28, 32, 40, 48, 56, 64, 96  | 
| index\_metric |  The metric to measure the distance between points when finding nearest neighbors\. When training with `index_type` set to `faiss.IVFPQ`, the `INNER_PRODUCT` distance and `COSINE` similarity are not supported\. **Optional**  Valid values: *L2* for Euclidean\-distance, *INNER\_PRODUCT* for inner\-product distance, *COSINE* for cosine similarity\. Default value: *L2*  | 
| index\_type |  The type of index\. **Optional** Valid values: *faiss\.Flat*, *faiss\.IVFFlat*, *faiss\.IVFPQ*\. Default values: *faiss\.Flat*  | 
| mini\_batch\_size |  The number of observations per mini\-batch for the data iterator\.  **Optional** Valid values: positive integer Default value: 5000  | 