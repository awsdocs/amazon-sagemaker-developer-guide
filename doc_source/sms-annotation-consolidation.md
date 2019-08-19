# Annotation Consolidation<a name="sms-annotation-consolidation"></a>

Annotation consolidation combines the annotations of two or more workers into a single label for your data objects\. An *annotation* is the result of a single worker\. *Annotation consolidation* combines multiple annotations from different workers to come up with a probabilistic estimate of what the true label should be\. The *label* is assigned to each object in the dataset\. Each object in the dataset typically has multiple annotations but only one label or set of labels\.

You can decide how many workers should annotate each object in your dataset\. More workers can increase the accuracy of your labels but also increases the cost of labeling\. Amazon SageMaker Ground Truth uses the following defaults in the Amazon SageMaker console\. When you use the [CreateLabelingJob](API_CreateLabelingJob.md) operation, you set the number of workers that should annotate each data object using the `NumberOfHumanWorkersPerDataObject` parameter\.
+ **Text classification**—3 workers
+ **Image classification**—3 workers
+ **Bounding boxes**—5 workers
+ **Semantic segmentation**—3 workers
+ **Named entity recognition**—3 workers

You can override the default number of workers that label a data object using the console or the [CreateLabelingJob](API_CreateLabelingJob.md) operation\.

Ground Truth provides an annotation consolidation function for each of its predefined labeling tasks: Bounding box, image classification, semantic segmentation, and text classification\. These are the functions:
+ Multi\-class annotation consolidation for image and text classification uses a variant of the Expectation Maximization approach to annotations\. It estimates parameters for each worker and uses Bayesian inference to estimate the true class based on the class annotations from individual workers\.
+ Bounding box annotation consolidates bounding boxes from multiple workers\. This function finds the most similar boxes from different workers based on the Jaccard index, or intersection over union, of the boxes and averages them\.
+ Semantic segmentation annotation consolidation treats each pixel in a single image as a multi\-class classification\. This function treats the pixel annotations from workers as "votes," with more information from surrounding pixels incorporated by applying a smoothing function to the image\.
+ Named entity recognition clusters text selections by Jaccard similarity and calculates selection boundaries based on the mode, or median if the mode isn't clear\. The label resolves to the most assigned entity label in the cluster, breaking ties by random selection\.

**Note**  
If you want to run worker responses through different algorithms on your own, that data is stored in the `[project-name]/annotations/worker-response` folder of the Amazon S3 bucket where you direct the job output\.

## Creating Your Own Annotation Consolidation Function<a name="consolidation-lambda"></a>

There are many possible approaches for writing an annotation consolidation function\. The approach that you take depends on the nature of the annotations to consolidate\. Broadly, consolidation functions look at the annotations from workers, measure the similarity between them, and then use some form of probabilistic judgment to determine what the most probable label should be\.

### Assessing Similarity<a name="consolidation-assessing"></a>

To assess the similarity between labels, you can use one of the following strategies or you can use one that meets your data labeling needs:
+ For label spaces that consist of discrete, mutually exclusive categories, such as multi\-class classification, assessing similarity can be straightforward\. Discrete labels either match or not\. 
+ For label spaces that don't have discrete values, such as bounding box annotations, find a broad measure of similarity\. For bounding boxes, one such measure is the Jaccard index\. This measures the ratio of the intersection of two boxes with the union of the boxes to assess how similar they are\. For example, if there are three annotations, then there can be a function that determines whichannotations represent the same object and should be consolidated\.

### Assessing the Most Probable Label<a name="consolidation-probable-label"></a>

With one of the above strategies in mind, make some sort of probabilistic judgment on what the consolidated label should be\. In the case of discrete, mutually exclusive categories this can be straightforward\. One of the most common ways to do this is to take the results of a majority vote between the annotations\. This weights the annotations equally\. 

Some approaches attempt to estimate the accuracy of different annotators and weight their annotations in proportion to the probability of correctness\. An example of this is the Expectation Maximization method, which is used in the default Ground Truth consolidation function for multi\-class annotations\. 

For more information about creating an annotation consolidation function, see [Step 3: Processing with AWS Lambda](sms-custom-templates-step3.md)\.