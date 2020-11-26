# K\-Means Algorithm<a name="k-means"></a>

K\-means is an unsupervised learning algorithm\. It attempts to find discrete groupings within data, where members of a group are as similar as possible to one another and as different as possible from members of other groups\. You define the attributes that you want the algorithm to use to determine similarity\. 

Amazon SageMaker uses a modified version of the web\-scale k\-means clustering algorithm\. Compared with the original version of the algorithm, the version used by Amazon SageMaker is more accurate\. Like the original algorithm, it scales to massive datasets and delivers improvements in training time\. To do this, the version used by Amazon SageMaker streams mini\-batches \(small, random subsets\) of the training data\. For more information about mini\-batch k\-means, see [Web\-scale k\-means Clustering](https://www.eecs.tufts.edu/~dsculley/papers/fastkmeans.pdf)\.

The k\-means algorithm expects tabular data, where rows represent the observations that you want to cluster, and the columns represent attributes of the observations\. The *n* attributes in each row represent a point in *n*\-dimensional space\. The Euclidean distance between these points represents the similarity of the corresponding observations\. The algorithm groups observations with similar attribute values \(the points corresponding to these observations are closer together\)\. For more information about how k\-means works in Amazon SageMaker, see [How K\-Means Clustering Works](algo-kmeans-tech-notes.md)\.

**Topics**
+ [Input/Output Interface for the K\-Means Algorithm](#km-inputoutput)
+ [EC2 Instance Recommendation for the K\-Means Algorithm](#km-instances)
+ [K\-Means Sample Notebooks](#kmeans-sample-notebooks)
+ [How K\-Means Clustering Works](algo-kmeans-tech-notes.md)
+ [K\-Means Hyperparameters](k-means-api-config.md)
+ [Tune a K\-Means Model](k-means-tuning.md)
+ [K\-Means Response Formats](km-in-formats.md)

## Input/Output Interface for the K\-Means Algorithm<a name="km-inputoutput"></a>

For training, the k\-means algorithm expects data to be provided in the *train* channel \(recommended `S3DataDistributionType=ShardedByS3Key`\), with an optional *test* channel \(recommended `S3DataDistributionType=FullyReplicated`\) to score the data on\. Both `recordIO-wrapped-protobuf` and `CSV` formats are supported for training\. You can use either File mode or Pipe mode to train models on data that is formatted as `recordIO-wrapped-protobuf` or as `CSV`\.

For inference, `text/csv`, `application/json`, and `application/x-recordio-protobuf` are supported\. k\-means returns a `closest_cluster` label and the `distance_to_cluster` for each observation\.

For more information on input and output file formats, see [K\-Means Response Formats](km-in-formats.md) for inference and the [K\-Means Sample Notebooks](#kmeans-sample-notebooks)\. The k\-means algorithm does not support multiple instance learning, in which the training set consists of labeled “bags”, each of which is a collection of unlabeled instances\.

## EC2 Instance Recommendation for the K\-Means Algorithm<a name="km-instances"></a>

We recommend training k\-means on CPU instances\. You can train on GPU instances, but should limit GPU training to `p*.xlarge` instances because only one GPU per instance is used\.

## K\-Means Sample Notebooks<a name="kmeans-sample-notebooks"></a>

For a sample notebook that uses the SageMaker K\-means algorithm to segment the population of counties in the United States by attributes identified using principle component analysis, see [Analyze US census data for population segmentation using Amazon SageMaker](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_applying_machine_learning/US-census_population_segmentation_PCA_Kmeans/sagemaker-countycensusclustering.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. To open a notebook, click on its **Use** tab and select **Create copy**\.