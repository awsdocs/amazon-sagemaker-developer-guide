# Bounding Box<a name="sms-bounding-box"></a>

The images used to train a machine learning model often contain more than one object\. To classify and localize one or more objects within images, use the Amazon SageMaker Ground Truth bounding box labeling job task type\. In this context, localization means the pixel\-location of the bounding box\. For example, the output manifest file of a successfully completed single\-class bounding box task will contain the following: 

```
[
  {
    "boundingBox": {
      "boundingBoxes": [
        {
          "height": 2833,
          "label": "bird",
          "left": 681,
          "top": 599,
          "width": 1364
        }
      ],
      "inputImageProperties": {
        "height": 3726,
        "width": 2662
      }
    }
  }
]
```

The `boundingBoxes` parameter identifies the location of the bounding box drawn around an object identified as a "bird" relative to the top\-left corner of the image which is taken to be the \(0,0\) pixel\-coordinate\. In the previous example, **`left`** and **`top`** identify the location of the pixel in the top\-left corner of the bounding box relative to the top\-left corner of the image\. The dimensions of the bounding box are identified with **`height`** and **`width`**\. The `inputImageProperties` parameter gives the pixel\-dimensions of the original input image\.

When you use the bounding box task type, you can create single\- and multi\-class bounding box labeling jobs\. The output manifest file of a successfully completed multi\-class bounding box will contain the following: 

```
[
  {
    "boundingBox": {
      "boundingBoxes": [
        {
          "height": 938,
          "label": "squirrel",
          "left": 316,
          "top": 218,
          "width": 785
        },
        {
          "height": 825,
          "label": "rabbit",
          "left": 1930,
          "top": 2265,
          "width": 540
        },
        {
          "height": 1174,
          "label": "bird",
          "left": 748,
          "top": 2113,
          "width": 927
        },
        {
          "height": 893,
          "label": "bird",
          "left": 1333,
          "top": 847,
          "width": 736
        }
      ],
      "inputImageProperties": {
        "height": 3726,
        "width": 2662
      }
    }
  }
]
```

To learn more about the output manifest file that results from a bounding box labeling job, see [Bounding Box Job Output](sms-data-output.md#sms-output-box)\.

## Creating a Bounding Box Labeling Job<a name="sms-creating-bounding-box-labeling-job"></a>

You create a bounding box labeling job using the Ground Truth section of the Amazon SageMaker console or the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. Ground Truth provides a worker console similar to the following for labeling tasks\. When you create the labeling job with the console, you can modify the images and content that are shown in the following image\. If you create a labeling job using the API, you must supply a custom\-built template\. To learn how to create a custom template, see [Creating Custom Labeling Workflows](sms-custom-templates.md)\. To see examples of custom templates that can be used for bounding box labeling job types, see this [GitHub Repository](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/tree/master/images)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/box-bounding-example.png)

To learn how to start a bounding box labeling job in the console, see [Getting started](sms-getting-started.md)\. To use the API, see [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\.