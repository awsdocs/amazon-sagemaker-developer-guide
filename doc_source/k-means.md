# K\-Means Algorithm<a name="k-means"></a>

K\-means is an unsupervised learning algorithm\. It attempts to find discrete groupings within data, where members of a group are as similar as possible to one another and as different as possible from members of other groups\. You define the attributes that you want the algorithm to use to determine similarity\. 

Amazon SageMaker uses a modified version of the web\-scale k\-means clustering algorithm\. Compared with the original version of the algorithm, the version used by Amazon SageMaker is more accurate\. Like the original algorithm, it scales to massive datasets and delivers improvements in training time\. To do this, the version used by Amazon SageMaker streams mini\-batches \(small, random subsets\) of the training data\. For more information about mini\-batch k\-means, see [Web\-scale k\-means Clustering](https://www.eecs.tufts.edu/~dsculley/papers/fastkmeans.pdf)\.

The k\-means algorithm expects tabular data, where rows represent the observations that you want to cluster, and the columns represent attributes of the observations\. The *n* attributes in each row represent a point in *n*\-dimensional space\. The Euclidean distance between these points represents the similarity of the corresponding observations\. The algorithm groups observations with similar attribute values \(the points corresponding to these observations are closer together\)\. For more information about how k\-means works in Amazon SageMaker, see [How K\-Means Clustering Works](algo-kmeans-tech-notes.md)\.

## Input/Output Interface<a name="km-inputoutput"></a>

The k\-means algorithm expects data to be provided in the *train* channel \(recommended S3DataDistributionType=ShardedByS3Key\), with an optional *test* channel \(recommended S3DataDistributionType=FullyReplicated\) to score the data on\. Both `recordIO-wrapped-protobuf` and `CSV` formats are supported for training\. k\-means can be trained in File or Pipe mode when using recordIO\-wrapped protobuf, but only in File mode for the `CSV` format\.

For inference, `text/csv`, `application/json`, and `application/x-recordio-protobuf` are supported\. k\-means returns a `closest_cluster` label and the `distance_to_cluster` for each observation\.

Please see the example notebooks for more details on k\-means data formats\.

## EC2 Instance Recommendation<a name="km-instances"></a>

We recommend training k\-means on CPU instances\. You can train on GPU instances, but should limit GPU training to `p*.xlarge` instances because only one GPU per instance is used\.