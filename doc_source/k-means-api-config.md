# K\-Means Hyperparameters<a name="k-means-api-config"></a>

In the [CreateTrainingJob](API_CreateTrainingJob.md) request, you specify the training algorithm that you want to use\. You can also specify algorithm\-specific hyperparameters as string\-to\-string maps\. The following table lists the hyperparameters for the k\-means training algorithm provided by Amazon SageMaker\. For more information about how k\-means clustering works, see [How K\-Means Clustering Works](algo-kmeans-tech-notes.md)\.


| Parameter Name | Description | 
| --- | --- | 
| k | Number of required clusters \(also known as *k*\)\. Required\.  Valid values: positive integer Default value: \-  | 
| feature\_dim | Dimension of the input vectors\. Required\.  Valid values: positive integer Default value: \-  | 
| init\_method | The method by which we choose the initial centers\. Optional\.  Valid values: Either `random` or `kmeans++`\. Default value: `random`  | 
| mini\_batch\_size | Number of examples in a mini\-batch\. Required\.  Valid values: positive integer Default value: 5000  | 
| extra\_center\_factor | The algorithm creates `num_clusters` \* `extra_center_factor` as it runs and reduces the number of centers to `k` when finalizing\. Optional\. Valid values: Either a positive integer or `auto`\. Default value: `auto`  | 
| local\_lloyd\_max\_iter | Maximum iterations for Lloyds EM procedure in the local kmeans used in the finalize stage\. Optional\.  Valid values: positive integer Default value: 300  | 
| local\_lloyd\_tol | Tolerance for change in ssd for early stopping in local kmeans\. Optional\.  Valid values: Float\. Range in \[0, 1\]\. Default value: 0\.0001  | 
| local\_lloyd\_init\_method | Initialization method for local version\. Optional\.  Valid values: Either `random` or `kmeans++`\. Default value: `kmeans++`  | 
| local\_lloyd\_num\_trials | Local version is run multiple times and the one with the best loss is chosen\. This determines how many times\. Optional\.  Valid values: Either a positive integer or `auto`\. Default value: `auto`  | 
| half\_life\_time\_size | The points can have a decayed weight\. When a point is observed its weight, with regard to the computation of the cluster mean is 1\. This weight decays exponentially as we observe more points\. The exponent coefficient is chosen so that after observing `half_life_time_size` points after the mentioned point, its weight will become 1/2\. If set to 0, there is no decay\. Optional\.  Valid values: non\-negative integer Default value: 0  | 
| epochs | Number of passes done over the training data\. Optional\.  Valid values: positive integer Default value: 1  | 
| eval\_metrics | Optional\.  Valid values: Either *msd* or *ssd*\. Default value: *msd*  | 