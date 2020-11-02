# Consolidate Annotations<a name="sms-annotation-consolidation"></a>

An *annotation* is the result of a single worker's labeling task\. *Annotation consolidation* combines the annotations of two or more workers into a single label for your data objects\. A label, which is assigned to each object in the dataset, is a probabilistic estimate of what the true label should be\. Each object in the dataset typically has multiple annotations, but only one label or set of labels\.

You can decide how many workers should annotate each object in your dataset\. Using more workers can increase the accuracy of your labels, but also increases the cost of labeling\. If you use the Amazon SageMaker console to create a labeling job, the following are the defaults for the number of workers who can annotate objects: 
+ Text classification—3 workers
+ Image classification—3 workers
+ Bounding boxes—5 workers
+ Semantic segmentation—3 workers
+ Named entity recognition—3 workers

When you use the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation, you set the number of workers to annotate each data object with the `NumberOfHumanWorkersPerDataObject` parameter\. You can override the default number of workers that annotate a data object using the console or the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\.

Ground Truth provides an annotation consolidation function for each of its predefined labeling tasks: bounding box, image classification, name entity recognition, semantic segmentation, and text classification\. These are the functions:
+ Multi\-class annotation consolidation for image and text classification uses a variant of the [Expectation Maximization](https://en.wikipedia.org/wiki/Expectation-maximization_algorithm) approach to annotations\. It estimates parameters for each worker and uses Bayesian inference to estimate the true class based on the class annotations from individual workers\. 
+ Bounding box annotation consolidates bounding boxes from multiple workers\. This function finds the most similar boxes from different workers based on the [Jaccard index](https://en.wikipedia.org/wiki/Jaccard_index), or intersection over union, of the boxes and averages them\. 
+ Semantic segmentation annotation consolidation treats each pixel in a single image as a multi\-class classification\. This function treats the pixel annotations from workers as "votes," with more information from surrounding pixels incorporated by applying a smoothing function to the image\.
+ Named entity recognition clusters text selections by Jaccard similarity and calculates selection boundaries based on the mode, or the median if the mode isn't clear\. The label resolves to the most assigned entity label in the cluster, breaking ties by random selection\.

You can use other algorithms to consolidate annotations\. For information, see [Create Your Own Annotation Consolidation Function](#consolidation-lambda)\. 

## Create Your Own Annotation Consolidation Function<a name="consolidation-lambda"></a>

You can choose to use your own annotation consolidation function to determine the final labels for your labeled objects\. There are many possible approaches for writing a function and the approach that you take depends on the nature of the annotations to consolidate\. Broadly, consolidation functions look at the annotations from workers, measure the similarity between them, and then use some form of probabilistic judgment to determine what the most probable label should be\.

If you want to use other algorithms to create annotation consolidations functions, you can find the worker responses in the `[project-name]/annotations/worker-response` folder of the Amazon S3 bucket where you direct the job output\.

### Assess Similarity<a name="consolidation-assessing"></a>

To assess the similarity between labels, you can use one of the following strategies, or you can use one that meets your data labeling needs:
+ For label spaces that consist of discrete, mutually exclusive categories, such as multi\-class classification, assessing similarity can be straightforward\. Discrete labels either match or do not match\. 
+ For label spaces that don't have discrete values, such as bounding box annotations, find a broad measure of similarity\. For bounding boxes, one such measure is the Jaccard index\. This measures the ratio of the intersection of two boxes with the union of the boxes to assess how similar they are\. For example, if there are three annotations, then there can be a function that determines which annotations represent the same object and should be consolidated\.

### Assess the Most Probable Label<a name="consolidation-probable-label"></a>

With one of the strategies detailed in the previous sections in mind, make some sort of probabilistic judgment on what the consolidated label should be\. In the case of discrete, mutually exclusive categories, this can be straightforward\. One of the most common ways to do this is to take the results of a majority vote between the annotations\. This weights the annotations equally\. 

Some approaches attempt to estimate the accuracy of different annotators and weight their annotations in proportion to the probability of correctness\. An example of this is the Expectation Maximization method, which is used in the default Ground Truth consolidation function for multi\-class annotations\. 

For more information about creating an annotation consolidation function, see [Step 3: Processing with AWS Lambda](sms-custom-templates-step3.md)\.