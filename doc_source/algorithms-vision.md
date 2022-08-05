# Built\-in SageMaker Algorithms for Computer Vision<a name="algorithms-vision"></a>

SageMaker provides image processing algorithms that are used for image classification, object detection, and computer vision\.
+ [Image Classification Algorithm](image-classification.md)—uses example data with answers \(referred to as a *supervised algorithm*\)\. Use this algorithm to classify images\.
+ [Object Detection Algorithm](object-detection.md)—detects and classifies objects in images using a single deep neural network\. It is a supervised learning algorithm that takes images as input and identifies all instances of objects within the image scene\.
+ [Semantic Segmentation Algorithm](semantic-segmentation.md)—provides a fine\-grained, pixel\-level approach to developing computer vision applications\.


| Algorithm name | Channel name | Training input mode | File type | Instance class | Parallelizable | 
| --- | --- | --- | --- | --- | --- | 
| Image Classification | train and validation, \(optionally\) train\_lst, validation\_lst, and model | File or Pipe | recordIO or image files \(\.jpg or \.png\)  | GPU | Yes | 
| Object Detection | train and validation, \(optionally\) train\_annotation, validation\_annotation, and model | File or Pipe | recordIO or image files \(\.jpg or \.png\)  | GPU | Yes | 
| Semantic Segmentation | train and validation, train\_annotation, validation\_annotation, and \(optionally\) label\_map and model | File or Pipe | Image files | GPU \(single instance only\) | No | 