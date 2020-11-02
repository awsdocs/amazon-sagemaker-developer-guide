# Object Detection Request and Response Formats<a name="object-detection-in-formats"></a>

## Request Format<a name="object-detection-json"></a>

Query a trained model by using the model's endpoint\. The endpoint takes \.jpg and \.png image formats with `image/jpeg` and `image/png` content\-types\.

## Response Formats<a name="object-detection-recordio"></a>

The response is the class index with a confidence score and bounding box coordinates for all objects within the image encoded in JSON format\. The following is an example of response \.json file:

```
{"prediction":[
  [4.0, 0.86419455409049988, 0.3088374733924866, 0.07030484080314636, 0.7110607028007507, 0.9345266819000244],
  [0.0, 0.73376623392105103, 0.5714187026023865, 0.40427327156066895, 0.827075183391571, 0.9712159633636475],
  [4.0, 0.32643985450267792, 0.3677481412887573, 0.034883320331573486, 0.6318609714508057, 0.5967587828636169],
  [8.0, 0.22552496790885925, 0.6152569651603699, 0.5722782611846924, 0.882301390171051, 0.8985623121261597],
  [3.0, 0.42260299175977707, 0.019305512309074402, 0.08386176824569702, 0.39093565940856934, 0.9574796557426453]
]}
```

Each row in this \.json file contains an array that represents a detected object\. Each of these object arrays consists of a list of six numbers\. The first number is the predicted class label\. The second number is the associated confidence score for the detection\. The last four numbers represent the bounding box coordinates \[xmin, ymin, xmax, ymax\]\. These output bounding box corner indices are normalized by the overall image size\. Note that this encoding is different than that use by the input \.json format\. For example, in the first entry of the detection result, 0\.3088374733924866 is the left coordinate \(x\-coordinate of upper\-left corner\) of the bounding box as a ratio of the overall image width, 0\.07030484080314636 is the top coordinate \(y\-coordinate of upper\-left corner\) of the bounding box as a ratio of the overall image height, 0\.7110607028007507 is the right coordinate \(x\-coordinate of lower\-right corner\) of the bounding box as a ratio of the overall image width, and 0\.9345266819000244 is the bottom coordinate \(y\-coordinate of lower\-right corner\) of the bounding box as a ratio of the overall image height\. 

To avoid unreliable detection results, you might want to filter out the detection results with low confidence scores\. In the [object detection sample notebook](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/introduction_to_amazon_algorithms/object_detection_pascalvoc_coco), we provide scripts to remove the low confidence detections\. Scripts are also provided to plot the bounding boxes on the original image\.

For batch transform, the response is in JSON format, where the format is identical to the JSON format described above\. The detection results of each image is represented as a JSON file\. For example:

```
{"prediction": [[label_id, confidence_score, xmin, ymin, xmax, ymax], [label_id, confidence_score, xmin, ymin, xmax, ymax]]}
```

For more details on training and inference, see the [Object Detection Sample Notebooks](object-detection.md#object-detection-sample-notebooks)\.

## OUTPUT: JSON Response Format<a name="object-detection-output-json"></a>

accept: application/json;annotation=1

```
{
   "image_size": [
      {
         "width": 500,
         "height": 400,
         "depth": 3
      }
   ],
   "annotations": [
      {
         "class_id": 0,
         "score": 0.943,
         "left": 111,
         "top": 134,
         "width": 61,
         "height": 128
      },
      {
         "class_id": 0,
         "score": 0.0013,
         "left": 161,
         "top": 250,
         "width": 79,
         "height": 143
      },
      {
         "class_id": 1,
         "score": 0.0133,
         "left": 101,
         "top": 185,
         "width": 42,
         "height": 130
      }
   ]
}
```