# SageMaker Clarify Computer Vision<a name="clarify-model-explainability-computer-vision"></a>

Amazon SageMaker Clarify generates heat maps for images that highlight features under analysis\. In particular, the heat maps provide insights into how your computer vision models classify the images and how they detect objects in those images\.

**Topics**
+ [Explain Image Classification with Amazon SageMaker Clarify](#clarify-model-explainability-computer-vision-image-classification)
+ [Explain Object Detection with Amazon SageMaker Clarify](#clarify-model-explainability-computer-vision-object-detection)

## Explain Image Classification with Amazon SageMaker Clarify<a name="clarify-model-explainability-computer-vision-image-classification"></a>

SageMaker Clarify processing jobs provides support for explaining images using the [KernelSHAP algorithm](https://arxiv.org/abs/1705.07874)\. This algorithm treats the image as a collection of super pixels\. Given a dataset consisting of images, the processing job outputs a dataset of images where each image shows the heat map of the relevant super pixels\.

For a sample notebook that uses SageMaker Clarify to classify images and explain its classification, see [Explaining Image Classification with SageMaker Clarify](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker_processing/computer_vision/explainability_image_classification.ipynb)\.

## Explain Object Detection with Amazon SageMaker Clarify<a name="clarify-model-explainability-computer-vision-object-detection"></a>

Amazon SageMaker Clarify can detect and classify objects in an image and then provide an explanation for the detection predicted\. The objects are first categorized into one of the classes in a specified collection\. SageMaker Clarify produces a confidence score for each object that it belongs to the class and the coordinates of a bounding box that delimits the object\. For each detected object identified with sufficient probability, SageMaker Clarify extracts features using the [Simple Linear Iterative Clustering \(SLIC\)](https://scikit-image.org/docs/dev/api/skimage.segmentation.html#skimage.segmentation.slic) method from scikit\-learn library for image segmentation\.

 SageMaker Clarify can then provide an explanation for the detection of an object in the image scene\. It uses Shapley values to determine the contribution that each feature has made to a model prediction\. It produces a heat map that shows how important each of the features in the image scene were for its detection of the object\. The context of an object is often very important for the correct identification of an object, so it is important not to just use the cropped image for classification\.

For a sample notebook that uses SageMaker Clarify to detect objects in an image and explain its predictions, see [Explaining object detection models with Amazon SageMaker Clarify](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-clarify/computer_vision/object_detection/object_detection_clarify.ipynb)\.