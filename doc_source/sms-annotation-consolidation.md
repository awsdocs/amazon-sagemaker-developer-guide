# Annotation Consolidation<a name="sms-annotation-consolidation"></a>

Annotation consolidation combines the annotations of two or more workers into a single label for your data objects\. An *annotation* is the result of a single worker\. *Annotation consolidation* combines multiple annotations from different workers to come up with a probabilistic estimate of what the true label should be\. The *label* is assigned to each object in the dataset\. Each object in the dataset will typically have multiple annotations but only one label or set of labels\.

You can decide how many workers should annotate each object in your dataset\. More workers can increase the accuracy of your labels but also increases the cost of labeling the data\. Amazon SageMaker Ground Truth uses the following defaults in the Amazon SageMaker console\. When you use the [CreateLabelingJob](API_CreateLabelingJob.md) operation you set the number of workers that should annotate each data object using the `NumberOfHumanWorkersPerDataObject` parameter\.
+ **Text classification**—3 workers
+ **Image classification**—3 workers
+ **Bounding boxes**—5 workers
+ **Semantic segmentation**—3 workers

You can override the default number of workers that label a data object using the console or the [CreateLabelingJob](API_CreateLabelingJob.md) operation\.

Ground Truth provides an annotation consolidation function for each of its predefined labeling tasks: bounding box, image classification, semantic segmentation, and text classification\.
+ Multi\-class annotation consolidation for image and text classification uses a variant of the Expectation Maximization approach to annotations\. It estimates parameters for each worker and uses Bayesian inference to estimate the true class based on the class annotations from individual workers\.
+ Bounding box annotation consolidates bounding boxes from multiple workers by finding the most similar boxes from different workers based on the Jaccard index, or intersection over union, of the boxes and averaging them\.
+ Semantic segmentation annotation consolidation treats each pixel in a single image as a multi\-class classification, and treats the pixel annotations from workers as "votes", with additional information from surrounding pixels incorporated by applying a smoothing function to the image\.

## Creating Your Own Annotation Consolidation Function<a name="consolidation-lambda"></a>

There are many possible approaches for writing an annotation consolidation function\. The approach that you take depends on the nature of the annotations to consolidate\. Broadly, consolidation functions should look at the annotations from workers, measure the similarity between them, and then use some form of probabilistic judgment to determine what the most probable label should be\.

### Assessing Similarity<a name="consolidation-assessing"></a>

To assess the similarity between labels, you can use one of the following strategies or you can use one that meets your data labeling needs:
+ For label spaces that consist of discrete, mutually exclusive categories, such as multi\-class classification, assessing similarity can be straightforward\. Discrete labels either match or they don't\. 
+ For label spaces that don't have discrete values, such as bounding box annotations, you instead need to find some broader measure of similarity\. In the case of bounding boxes, one such measure is the Jaccard index\. This measures the ratio of the intersection of two boxes with the union of the boxes to assess how similar they are\. For example, if there are 3 annotators there needs to be some function, based on similarity, to determine which boxes represent the same object and should be consolidated together\.

### Assessing the Most Probable Label<a name="consolidation-probable-label"></a>

Next you need to make some sort of probabilistic judgement on what the consolidated label should be\. In the case of discrete, mutually exclusive categories this can be straightforward\. One of the most common ways to do this is to take the results of a majority vote between the annotations\. This weights the annotations equally\. 

More sophisticated approaches, such as the Expectation Maximization method used in the default Ground Truth consolidation function for multi\-class annotations, attempt to estimate the accuracy of different annotators and weight their annotations in proportion to the probability of correctness\. 

For more information about creating an annotation consolidation function, see [Step 3: Processing with AWS Lambda](sms-custom-templates-step3.md)\.