# Built\-in SageMaker Algorithms for Computer Vision<a name="algorithms-vision"></a>

SageMaker provides image processing algorithms that are used for image classification, object detection, and computer vision\.
+ [Image Classification \- MXNet](image-classification.md)—uses example data with answers \(referred to as a *supervised algorithm*\)\. Use this algorithm to classify images\.
+ [Image Classification \- TensorFlow](image-classification-tensorflow.md)—uses pretrained TensorFlow Hub models to fine\-tune for specific tasks \(referred to as a *supervised algorithm*\)\. Use this algorithm to classify images\.
+ [Object Detection](object-detection.md)—detects and classifies objects in images using a single deep neural network\. It is a supervised learning algorithm that takes images as input and identifies all instances of objects within the image scene\.
+ [Semantic Segmentation Algorithm](semantic-segmentation.md)—provides a fine\-grained, pixel\-level approach to developing computer vision applications\.


| Algorithm name | Channel name | Training input mode | File type | Instance class | Parallelizable | 
| --- | --- | --- | --- | --- | --- | 
| Image Classification \- MXNet | train and validation, \(optionally\) train\_lst, validation\_lst, and model | File or Pipe | recordIO or image files \(\.jpg or \.png\)  | GPU | Yes | 
| Image Classification \- TensorFlow | training and validation | File | image files \(\.jpg, \.jpeg, or \.png\)  | CPU or GPU | Yes \(only across multiple GPUs on a single instance\) | 
| Object Detection | train and validation, \(optionally\) train\_annotation, validation\_annotation, and model | File or Pipe | recordIO or image files \(\.jpg or \.png\)  | GPU | Yes | 
| Semantic Segmentation | train and validation, train\_annotation, validation\_annotation, and \(optionally\) label\_map and model | File or Pipe | Image files | GPU \(single instance only\) | No | 