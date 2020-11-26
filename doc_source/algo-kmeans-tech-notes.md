# How K\-Means Clustering Works<a name="algo-kmeans-tech-notes"></a>

K\-means is an algorithm that trains a model that groups similar objects together\. The k\-means algorithm accomplishes this by mapping each observation in the input dataset to a point in the *n*\-dimensional space \(where *n* is the number of attributes of the observation\)\. For example, your dataset might contain observations of temperature and humidity in a particular location, which are mapped to points \(*t, h*\) in 2\-dimensional space\. 

**Note**  
Clustering algorithms are unsupervised\. In unsupervised learning, labels that might be associated with the objects in the training dataset aren't used\. 

In k\-means clustering, each cluster has a center\. During model training, the k\-means algorithm uses the distance of the point that corresponds to each observation in the dataset to the cluster centers as the basis for clustering\. You choose the number of clusters \(*k*\) to create\. 

For example, suppose that you want to create a model to recognize handwritten digits and you choose the MNIST dataset for training\. The dataset provides thousands of images of handwritten digits \(0 through 9\)\. In this example, you might choose to create 10 clusters, one for each digit \(0, 1, …, 9\)\. As part of model training, the k\-means algorithm groups the input images into 10 clusters\.

Each image in the MNIST dataset is a 28x28\-pixel image, with a total of 784 pixels\. Each image corresponds to a point in a 784\-dimensional space, similar to a point in a 2\-dimensional space \(x,y\)\. To find a cluster to which a point belongs, the k\-means algorithm finds the distance of that point from all of the cluster centers\. It then chooses the cluster with the closest center as the cluster to which the image belongs\. 

**Note**  
Amazon SageMaker uses a customized version of the algorithm where, instead of specifying that the algorithm create *k* clusters, you might choose to improve model accuracy by specifying extra cluster centers *\(K = k\*x\)*\. However, the algorithm ultimately reduces these to *k* clusters\.

In SageMaker, you specify the number of clusters when creating a training job\. For more information, see [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\. In the request body, you add the `HyperParameters` string map to specify the `k` and `extra_center_factor` strings\.

The following is a summary of how k\-means works for model training in SageMaker:

1. It determines the initial *K* cluster centers\. 
**Note**  
In the following topics, *K* clusters refer to *k \* x*, where you specify *k* and *x* when creating a model training job\. 

1. It iterates over input training data and recalculates cluster centers\.

1. It reduces resulting clusters to *k* \(if the data scientist specified the creation of *k\*x* clusters in the request\)\. 

The following sections also explain some of the parameters that a data scientist might specify to configure a model training job as part of the `HyperParameters` string map\. 

**Topics**
+ [Step 1: Determine the Initial Cluster Centers](#kmeans-step1)
+ [Step 2: Iterate over the Training Dataset and Calculate Cluster Centers](#kmeans-step2)
+ [Step 3: Reduce the Clusters from *K* to *k*](#kmeans-step3)

## Step 1: Determine the Initial Cluster Centers<a name="kmeans-step1"></a>

When using k\-means in SageMaker, the initial cluster centers are chosen from the observations in a small, randomly sampled batch\. Choose one of the following strategies to determine how these initial cluster centers are selected:
+ The random approach—Randomly choose *K* observations in your input dataset as cluster centers\. For example, you might choose a cluster center that points to the 784\-dimensional space that corresponds to any 10 images in the MNIST training dataset\.

   
+ The k\-means\+\+ approach, which works as follows: 

  1. Start with one cluster and determine its center\. You randomly select an observation from your training dataset and use the point corresponding to the observation as the cluster center\. For example, in the MNIST dataset, randomly choose a handwritten digit image\. Then choose the point in the 784\-dimensional space that corresponds to the image as your cluster center\. This is cluster center 1\.

  1. Determine the center for cluster 2\. From the remaining observations in the training dataset, pick an observation at random\. Choose one that is different than the one you previously selected\. This observation corresponds to a point that is far away from cluster center 1\. Using the MNIST dataset as an example, you do the following:
     + For each of the remaining images, find the distance of the corresponding point from cluster center 1\. Square the distance and assign a probability that is proportional to the square of the distance\. That way, an image that is different from the one that you previously selected has a higher probability of getting selected as cluster center 2\. 
     + Choose one of the images randomly, based on probabilities assigned in the previous step\. The point that corresponds to the image is cluster center 2\.

  1. Repeat Step 2 to find cluster center 3\. This time, find the distances of the remaining images from cluster center 2\.

  1. Repeat the process until you have the *K* cluster centers\.

To train a model in SageMaker, you create a training job\. In the request, you provide configuration information by specifying the following `HyperParameters` string maps:
+ To specify the number of clusters to create, add the `k` string\.
+ For greater accuracy, add the optional `extra_center_factor` string\. 
+ To specify the strategy that you want to use to determine the initial cluster centers, add the `init_method` string and set its value to `random` or `k-means++`\.

For more information, see [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\. For an example, see [Create and Run a Training Job \(AWS SDK for Python \(Boto3\)\)](ex1-train-model.md#ex1-train-model-create-training-job)\. 

You now have an initial set of cluster centers\. 

## Step 2: Iterate over the Training Dataset and Calculate Cluster Centers<a name="kmeans-step2"></a>

The cluster centers that you created in the preceding step are mostly random, with some consideration for the training dataset\. In this step, you use the training dataset to move these centers toward the true cluster centers\. The algorithm iterates over the training dataset, and recalculates the *K* cluster centers\.

1. Read a mini\-batch of observations \(a small, randomly chosen subset of all records\) from the training dataset and do the following\. 
**Note**  
When creating a model training job, you specify the batch size in the `mini_batch_size` string in the `HyperParameters` string map\. 

   1. Assign all of the observations in the mini\-batch to one of the clusters with the closest cluster center\.

   1. Calculate the number of observations assigned to each cluster\. Then, calculate the proportion of new points assigned per cluster\.

      For example, consider the following clusters:

      Cluster c1 = 100 previously assigned points\. You added 25 points from the mini\-batch in this step\.

      Cluster c2 = 150 previously assigned points\. You added 40 points from the mini\-batch in this step\.

      Cluster c3 = 450 previously assigned points\. You added 5 points from the mini\-batch in this step\.

      Calculate the proportion of new points assigned to each of clusters as follows:

      ```
      p1 = proportion of points assigned to c1 = 25/(100+25)
      p2 = proportion of points assigned to c2 = 40/(150+40)
      p3 = proportion of points assigned to c3 = 5/(450+5)
      ```

   1. Compute the center of the new points added to each cluster:

      ```
      d1 = center of the new points added to cluster 1
      d2 = center of the new points added to cluster 2
      d3 = center of the new points added to cluster 3
      ```

   1. Compute the weighted average to find the updated cluster centers as follows:

      ```
      Center of cluster 1 = ((1 - p1) * center of cluster 1) + (p1 * d1)
      Center of cluster 2 = ((1 - p2) * center of cluster 2) + (p2 * d2)
      Center of cluster 3 = ((1 - p3) * center of cluster 3) + (p3 * d3)
      ```

1. Read the next mini\-batch, and repeat Step 1 to recalculate the cluster centers\. 

1. For more information about mini\-batch *k*\-means, see [Web\-Scale k\-means Clustering ](https://www.eecs.tufts.edu/~dsculley/papers/fastkmeans.pdf)\)\.

## Step 3: Reduce the Clusters from *K* to *k*<a name="kmeans-step3"></a>

If the algorithm created *K* clusters—*\(K = k\*x\)* where *x* is greater than 1—then it reduces the *K* clusters to *k* clusters\. \(For more information, see `extra_center_factor` in the preceding discussion\.\) It does this by applying Lloyd's method with `kmeans++` initialization to the *K* cluster centers\. For more information about Lloyd's method, see [k\-means clustering](https://pdfs.semanticscholar.org/0074/4cb7cc9ccbbcdadbd5ff2f2fee6358427271.pdf)\. 