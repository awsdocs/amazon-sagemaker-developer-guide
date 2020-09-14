# Output Data<a name="sms-data-output"></a>

The output from a labeling job is placed in the location that you specified in the console or in the call to the [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\.

Each line in the output data file is identical to the manifest file with the addition of an attribute and value for the label assigned to the input object\. The attribute name for the value is defined in the console or in the call to the `CreateLabelingJob` operation\. You can't use `-metadata` in the label attribute name\. If you are running an image semantic segmentation, 3D point cloud semantic segmentation, or 3D point cloud object tracking job, the label attribute must end with `-ref`\. For any other type of job, the attribute name can't end with `-ref`\.

The output of the labeling job is the value of the key\-value pair with the label\. The label and the value overwrites any existing JSON data in the input file with the new value\. 

For example, the following is the output from an image classification labeling job where the input data files were stored in an Amazon S3 `AWSDOC-EXAMPLE-BUCKET` and the label attribute name was defined as *`sport`*\. In this example the JSON object is formatted for readability, in the actual output file the JSON object is on a single line\. For more information about the data format, see [JSON Lines](http://jsonlines.org/)\. 

```
{
    "source-ref": "s3://AWSDOC-EXAMPLE-BUCKET/image_example.png",
    "sport":0,
    "sport-metadata":
    {
        "class-name": "football",
        "confidence": 0.00,
        "type":"groundtruth/image-classification",
        "job-name": "identify-sport",
        "human-annotated": "yes",
        "creation-date": "2018-10-18T22:18:13.527256"
    }
}
```

The value of the label can be any valid JSON\. In this case the label's value is the index of the class in the classification list\. Other job types, such as bounding box, have more complex values\.

Any key\-value pair in the input manifest file other than the label attribute is unchanged in the output file\. You can use this to pass data to your application\.

The output from a labeling job can be used as the input to another labeling job\. You can use this when you are chaining together labeling jobs\. For example, you can send one labeling job to determine the sport that is being played\. Then you send another using the same data to determine if the sport is being played indoors or outdoors\. By using the output data from the first job as the manifest for the second job, you can consolidate the results of the two jobs into one output file for easier processing by your applications\. 

The output data file is written to the output location periodically while the job is in progress\. These intermediate files contain one line for each line in the manifest file\. If an object is labeled, the label is included\. If the object hasn't been labeled, it is written to the intermediate output file identically to the manifest file\.

## Output Directories<a name="sms-output-directories"></a>

Ground Truth creates several directories in your Amazon S3 output path\. These directories contain the results of your labeling job and other artifacts of the job\. The top\-level directory for a labeling job is given the same name as your labeling job; the output directories are placed beneath it\. For example, if you named your labeling job **find\-people**, your output would be in the following directories:

```
s3://AWSDOC-EXAMPLE-BUCKET/find-people/activelearning
s3://AWSDOC-EXAMPLE-BUCKET/find-people/annotations
s3://AWSDOC-EXAMPLE-BUCKET/find-people/inference
s3://AWSDOC-EXAMPLE-BUCKET/find-people/manifests
s3://AWSDOC-EXAMPLE-BUCKET/find-people/training
```

Each directory contains the following output:

### Active Learning Directory<a name="sms-output-activelearning"></a>

The `activelearning` directory is only present when you are using automated data labeling\. It contains the input and output validation set for automated data labeling, and the input and output folder for automatically labeled data\.

### Annotations Directory<a name="sms-directories-annotations"></a>

The `annotations` directory contains all of the annotations made by the workforce\. These are the responses from individual workers that have not been consolidated into a single label for the data object\. 

There are three subdirectories in the `annotations` directory\. 

The first, `worker-response`, contains the responses from individual workers\. This contains a subdirectory for each iteration, which in turn contains a subdirectory for each data object in that iteration\. The annotation for each data object is stored in a timestamped JSON file\. There may be more than one annotation for each data object in this directory, depending on how many workers you want to annotate each object\.

The second, `consolidated-annotation`, contains information required to consolidate the annotations in the current batch into labels for your data objects\.

The third, `intermediate`, contains the output manifest for the current batch with any completed labels\. This file is updated as the label for each data object is completed\.

### Inference Directory<a name="sms-directories-inference"></a>

The `inference` directory is only present when you are using automated data labeling\. This directory contains the input and output files for the SageMaker batch transform used while labeling data objects\.

### Manifest Directory<a name="sms-directories-manifest"></a>

The `manifest` directory contains the output manifest from your labeling job\. There is one subdirectory in the manifest directory, `output`\. The `output` directory contains the output manifest file for your labeling job\. The file is named `output.manifest`\.

### Training Directory<a name="sms-directories-training"></a>

The `training` directory is only present when you are using automated data labeling\. This directory contains the input and output files used to train the automated data labeling model\.

## Confidence Score<a name="sms-output-confidence"></a>

When you have more than one worker annotate a single task, your label results from annotation conslidation\. Ground Truth calculates a confidence score for each label\. A *confidence score* is a number between 0 and 1 that indicates how confident Ground Truth is in the label\. You can use the confidence score to compare labeled data objects to each other, and to identify the least or most confident labels\.

You should not interpret the value of a confidence score as an absolute value, or compare confidence scores across labeling jobs\. For example, if all of the confidence scores are between 0\.98 and 0\.998, you should only compare the data objects with each other and not rely on the high confidence scores\. 

You should not compare the confidence scores of human\-labeled data objects and auto\-labeled data objects\. The confidence scores for humans are calculated using the annotation consolidation function for the task, while the confidence scores for automated labeling are calculated using a model that incorporates object features\. The two models generally have different scales and average confidence\.

For a bounding box labeling job, Ground Truth calculates a confidence score per box\. You can compare confidence scores within one image or across images for the same labeling type \(human or auto\)\. You can't compare confidence scores across labeling jobs\.

If a single worker annotates a task \(`NumberOfHumanWorkersPerDataObject` is set to `1` or in the console, you enter 1 for **Number of workers per dataset object**\), the confidence score is set to `0.00`\. 

## Output Metadata<a name="sms-output-metadata"></a>

The output from each job contains metadata about the label assigned to data objects\. These elements are the same for all jobs with minor variations\. The following example shows the metadata elements:

```
    "confidence": 0.00,
    "type": "groundtruth/image-classification",
    "job-name": "identify-animal-species",
    "human-annotated": "yes",
    "creation-date": "2020-10-18T22:18:13.527256"
```

The elements have the following meaning:
+ `confidence` – The confidence that Ground Truth has that the label is correct\. For more information, see [Confidence Score](#sms-output-confidence)\.
+ `type` – The type of classification job\. For job types, see [Built\-in Task Types](sms-task-types.md)\. 
+ `job-name` – The name assigned to the job when it was created\.
+ `human-annotated` – Whether the data object was labeled by a human or by automated data labeling\. For more information, see [Automate Data Labeling](sms-automated-labeling.md)\.
+ `creation-date` – The date and time that the label was created\.

## Classification Job Output<a name="sms-output-class"></a>

The following are sample outputs \(output manifest files\) from an image classification job and a text classification job\. They include the label that Ground Truth assigned to the data object, the value for the label, and metadata that describes the label\.

In addition to the standard metadata elements, the metadata for a classification job includes the text value of the label's class\. For more information, see [Image Classification Algorithm](image-classification.md)\.

The red, italicized text in the examples below depends on labeling job specifications and output data\. 

```
{
    "source-ref":"s3://AWSDOC-EXAMPLE-BUCKET/example_image.jpg",
    "species":"0",
    "species-metadata":
    {
        "class-name": "dog",
        "confidence": 0.00,
        "type": "groundtruth/image-classification",
        "job-name": "identify-animal-species",
        "human-annotated": "yes",
        "creation-date": "2018-10-18T22:18:13.527256"
    }
}
```

```
{
    "source":"The food was delicious",
    "mood":"1",
    "mood-metadata":
    {
        "class-name": "positive",
        "confidence": 0.8,
        "type": "groundtruth/text-classification",
        "job-name": "label-sentiment",
        "human-annotated": "yes",
        "creation-date": "2020-10-18T22:18:13.527256"
    }
}
```

## Multi\-label Classification Job Output<a name="sms-output-multi-label-classification"></a>

The following are example output manifest files from a multi\-label image classification job and a multi\-label text classification job\. They include the labels that Ground Truth assigned to the data object \(for example, the image or piece of text\) and metadata that describes the labels the worker saw when completing the labeling task\. 

The label attribute name parameter \(for example, `image-label-attribute-name`\) contains an array of all of the labels selected by at least one of the workers who completed this task\. This array contains integer keys \(for example, `[1,0,8]`\) that correspond to the labels found in `class-map`\. In the multi\-label image classification example, `bicycle`, `person`, and `clothing` were selected by at least one of the workers who completed the labeling task for the image, `exampleimage.jpg`\.

The `confidence-map` shows the confidence score that Ground Truth assigned to each label selected by a worker\. To learn more about Ground Truth confidence scores, see [Confidence Score](#sms-output-confidence)\.

The red, italicized text in the examples below depends on labeling job specifications and output data\. 

The following is an example of a multi\-label image classification output manifest file\. 

```
{
    "source-ref": "s3://AWSDOC-EXAMPLE-BUCKET/example_image.jpg",
    "image-label-attribute-name":[1,0,8],
    "image-label-attribute-name-metadata":
       {
        "job-name":"labeling-job/image-label-attribute-name",
        "class-map":
            {
                "1":"bicycle","0":"person","8":"clothing"
            },
        "human-annotated":"yes",
        "creation-date":"2020-02-27T21:36:25.000201",
        "confidence-map":
            {
                "1":0.95,"0":0.77,"8":0.2
            },
        "type":"groundtruth/image-classification-multilabel"
        }
}
```

The following is an example of a multi\-label text classification output manifest file\. In this example, `approving`, `sad` and `critical` were selected by at least one of the workers who completed the labeling task for the object `exampletext.txt` found in `AWSDOC-EXAMPLE-BUCKET`\.

```
{
    "source-ref": "AWSDOC-EXAMPLE-BUCKET/text_file.txt",
    "text-label-attribute-name":[1,0,4],
    "text-label-attribute-name-metadata":
       {
        "job-name":"labeling-job/text-label-attribute-name",
        "class-map":
            {
                "1":"approving","0":"sad","4":"critical"
            },
        "human-annotated":"yes",
        "creation-date":"2020-02-20T21:36:25.000201",
        "confidence-map":
            {
                "1":0.95,"0":0.77,"4":0.2
            },
        "type":"groundtruth/text-classification-multilabel"
        }
}
```

## Bounding Box Job Output<a name="sms-output-box"></a>

The following is sample output \(output manifest file\) from a bounding box job\. For this task, three bounding boxes are returned\. The label value contains information about the size of the image, and the location of the bounding boxes\.

The `class_id` element is the index of the box's class in the list of available classes for the task\. The `class-map` metadata element contains the text of the class\.

The metadata has a separate confidence score for each bounding box\. The metadata also includes the `class-map` element that maps the `class_id` to the text value of the class\. For more information, see [Object Detection Algorithm](object-detection.md)\.

The red, italicized text in the examples below depends on labeling job specifications and output data\. 

```
{
    "source-ref": "s3://AWSDOC-EXAMPLE-BUCKET/example_image.png",
    "bounding-box":
    {
        "image_size": [{ "width": 500, "height": 400, "depth":3}],
        "annotations":
        [
            {"class_id": 0, "left": 111, "top": 134,
                    "width": 61, "height": 128},
            {"class_id": 5, "left": 161, "top": 250,
                     "width": 30, "height": 30},
            {"class_id": 5, "left": 20, "top": 20,
                     "width": 30, "height": 30}
        ]
    },
    "bounding-box-metadata":
    {
        "objects":
        [
            {"confidence": 0.8},
            {"confidence": 0.9},
            {"confidence": 0.9}
        ],
        "class-map":
        {
            "0": "dog",
            "5": "bone"
        },
        "type": "groundtruth/object-detection",
        "human-annotated": "yes",
        "creation-date": "2018-10-18T22:18:13.527256",
        "job-name": "identify-dogs-and-toys"
    }
 }
```

The output of a bounding box adjustment job looks like the following JSON\. Note that the original JSON is kept intact and two new jobs are listed, each with “adjust\-” prepended to the original attribute’s name\. 

```
{
    "source-ref": "S3 bucket location",
    "bounding-box":
    {
        "image_size": [{ "width": 500, "height": 400, "depth":3}],
        "annotations":
        [
            {"class_id": 0, "left": 111, "top": 134,
                    "width": 61, "height": 128},
            {"class_id": 5, "left": 161, "top": 250,
                     "width": 30, "height": 30},
            {"class_id": 5, "left": 20, "top": 20,
                     "width": 30, "height": 30}
        ]
    },
    "bounding-box-metadata":
    {
        "objects":
        [
            {"confidence": 0.8},
            {"confidence": 0.9},
            {"confidence": 0.9}
        ],
        "class-map":
        {
            "0": "dog",
            "5": "bone"
        },
        "type": "groundtruth/object-detection",
        "human-annotated": "yes",
        "creation-date": "2018-10-18T22:18:13.527256",
        "job-name": "identify-dogs-and-toys"
    },
    "adjusted-bounding-box":
    {
        "image_size": [{ "width": 500, "height": 400, "depth":3}],
        "annotations":
        [
            {"class_id": 0, "left": 110, "top": 135,
                    "width": 61, "height": 128},
            {"class_id": 5, "left": 161, "top": 250,
                     "width": 30, "height": 30},
            {"class_id": 5, "left": 10, "top": 10,
                     "width": 30, "height": 30}
        ]
    },
    "adjusted-bounding-box-metadata":
    {
        "objects":
        [
            {"confidence": 0.8},
            {"confidence": 0.9},
            {"confidence": 0.9}
        ],
        "class-map":
        {
            "0": "dog",
            "5": "bone"
        },
        "type": "groundtruth/object-detection",
        "human-annotated": "yes",
        "creation-date": "2018-11-20T22:18:13.527256",
        "job-name": "adjust-bounding-boxes-on-dogs-and-toys",
        "adjustment-status": "adjusted"
    }
}
```

In this output, the job's `type` doesn't change, but an `adjustment-status` field is added\. This field has the value of `adjusted` or `unadjusted`\. If multiple workers have reviewed the object and at least one adjusted the label, the status is `adjusted`\.

## Label Verification Job Output<a name="sms-output-bounding-box-verification"></a>

The output \(output manifest file\) of a bounding box verification job looks different than the output of a bounding box annotation job\. That's because the workers have a different type of task\. They're not labeling objects, but evaluating the accuracy of prior labeling, making a judgment, and then providing that judgment and perhaps some comments\.

If human workers are verifying or adjusting prior bounding box labels, the output of a verification job would look like the following JSON\. The red, italicized text in the examples below depends on labeling job specifications and output data\. 

```
{
    "source-ref":"s3://AWSDOC-EXAMPLE-BUCKET/image_example.png",
    "bounding-box":
    {
        "image_size": [{ "width": 500, "height": 400, "depth":3}],
        "annotations":
        [
            {"class_id": 0, "left": 111, "top": 134,
                    "width": 61, "height": 128},
            {"class_id": 5, "left": 161, "top": 250,
                     "width": 30, "height": 30},
            {"class_id": 5, "left": 20, "top": 20,
                     "width": 30, "height": 30}
        ]
    },
    "bounding-box-metadata":
    {
        "objects":
        [
            {"confidence": 0.8},
            {"confidence": 0.9},
            {"confidence": 0.9}
        ],
        "class-map":
        {
            "0": "dog",
            "5": "bone"
        },
        "type": "groundtruth/object-detection",
        "human-annotated": "yes",
        "creation-date": "2018-10-18T22:18:13.527256",
        "job-name": "identify-dogs-and-toys"
    },
    "verify-bounding-box":"1",
    "verify-bounding-box-metadata":
    {
        "class-name": "bad",
        "confidence": 0.93,
        "type": "groundtruth/label-verification",
        "job-name": "verify-bounding-boxes",
        "human-annotated": "yes",
        "creation-date": "2018-11-20T22:18:13.527256",
        "worker-feedback": [
            {"comment": "The bounding box on the bird is too wide on the right side."},
            {"comment": "The bird on the upper right is not labeled."}
        ]
    }
}
```

Although the `type` on the original bounding box output was `groundtruth/object-detection`, the new `type` is `groundtruth/label-verification`\. Also note that the `worker-feedback` array provides worker comments\. If the worker doesn't provide comments, the empty fields are excluded during consolidation\.

## Semantic Segmentation Job Output<a name="sms-output-segmentation"></a>

The following is the output manifest file from a semantic segmentation labeling job\. The value of the label for this job is a reference to a PNG file in an Amazon S3 bucket\.

In addition to the standard elements, the metadata for the label includes a color map that defines which color is used to label the image, the class name associated with the color, and the confidence score for each color\. For more information, see [Semantic Segmentation Algorithm](semantic-segmentation.md)\.

The red, italicized text in the examples below depends on labeling job specifications and output data\. 

```
{
    "source-ref": "s3://AWSDOC-EXAMPLE-BUCKET/example_city_image.png",
    "city-streets-ref": "S3 bucket location",
    "city-streets-ref-metadata": {
      "internal-color-map": {
        "0": {
           "class-name": "BACKGROUND",
           "confidence": 0.9,
           "hex-color": "#ffffff"
        },
        "1": {
           "class-name": "buildings",
           "confidence": 0.9,
           "hex-color": "#2acf59"
        },
        "2":  {
           "class-name": "road",
           "confidence": 0.9,
           "hex-color": "#f28333"
       }
     },
     "type": "groundtruth/semantic-segmentation",
     "human-annotated": "yes",
     "creation-date": "2018-10-18T22:18:13.527256",
     "job-name": "label-city-streets",
     },
     "verify-city-streets":"1",
     "verify-city-streets-metadata":
     {
        "class-name": "bad",
        "confidence": 0.93,
        "type": "groundtruth/label-verification",
        "job-name": "verify-city-streets",
        "human-annotated": "yes",
        "creation-date": "2018-11-20T22:18:13.527256",
        "worker-feedback": [
            {"comment": "The mask on the leftmost building is assigned the wrong side of the road."},
            {"comment": "The curb of the road is not labeled but the instructions say otherwise."}
        ]
     }
}
```

Confidence is scored on a per\-image basis\. Confidence scores are the same across all classes within an image\. 

The output of a semantic segmentation adjustment job looks similar to the following JSON\.

```
{
    "source-ref": "s3://AWSDOC-EXAMPLE-BUCKET/example_city_image.png",
    "city-streets-ref": "S3 bucket location",
    "city-streets-ref-metadata": {
      "internal-color-map": {
        "0": {
           "class-name": "BACKGROUND",
           "confidence": 0.9,
           "hex-color": "#ffffff"
        },
        "1": {
           "class-name": "buildings",
           "confidence": 0.9,
           "hex-color": "#2acf59"
        },
        "2":  {
           "class-name": "road",
           "confidence": 0.9,
           "hex-color": "#f28333"
       }
     },
     "type": "groundtruth/semantic-segmentation",
     "human-annotated": "yes",
     "creation-date": "2018-10-18T22:18:13.527256",
     "job-name": "label-city-streets",
     },
     "adjusted-city-streets-ref": "s3://AWSDOC-EXAMPLE-BUCKET/example_city_image.png",
     "adjusted-city-streets-metadata": {
      "internal-color-map": {
        "0": {
           "class-name": "BACKGROUND",
           "confidence": 0.9,
           "hex-color": "#ffffff"
        },
        "1": {
           "class-name": "buildings",
           "confidence": 0.9,
           "hex-color": "#2acf59"
        },
        "2":  {
           "class-name": "road",
           "confidence": 0.9,
           "hex-color": "#f28333"
       }
     },
     "type": "groundtruth/semantic-segmentation",
     "human-annotated": "yes",
     "creation-date": "2018-11-20T22:18:13.527256",
     "job-name": "adjust-label-city-streets",
     }
}
```

## Video Frame Object Detection Output<a name="sms-output-video-object-detection"></a>

The following is the output manifest file from a video frame object detection labeling job\. The *red, italicized text* in the examples below depends on labeling job specifications and output data\.

In addition to the standard elements, the metadata includes a class map that lists each class that has at least one label in the sequence\. The metadata also includes `job-name` which is the name you assigned to the labeling job\. For adjustment tasks, If one or more bounding boxes were modified, there is an `adjustment-status` parameter in the metadata for audit workflows that is set to `adjusted`\. 

```
{
    "source-ref": "s3://DOC-EXAMPLE-BUCKET/example-path/input-manifest.json",
    "CarObjectDetection-ref": "s3://AWSDOC-EXAMPLE-BUCKET/output/labeling-job-name/annotations/consolidated-annotation/output/0/SeqLabel.json",
    "CarObjectDetection-ref-metadata": {
        "class-map": {
            "0": "car"
            "1": "bus"
        },
        "job-name": "labeling-job/labeling-job-name",
        "human-annotated": "yes",
        "creation-date": "2020-05-15T08:01:16+0000",
        "type": "groundtruth/video-object-detection"
        }
}
```

Ground Truth creates one output sequence file for each sequence of video frames that was labeled\. Each output sequence file contains the following: 
+ All annotations for all frames in a sequence in the `detection-annotations` list of JSON objects\. 
+ For each frame that was annotated by a worker, the frame file name \(`frame`\), number \(`frame-no`\) and an `annotations` list of JSON objects\. 

  Each JSON object contains information about a single bounding box and associated label:
  + Box dimensions: `height` and `width` 
  + Box pixel location: `top` and `left`
  + Values of any `label-category-attributes` that were specified for that label\. 
  + The `class-id` of the box\. Use the `class-map` in the output manifest file to see which label category this ID maps to\. 

The following is an example of a `SeqLabel.json`\. 

```
{
    "detection-annotations": [
        {
            "annotations": [
                {
                    "height": 41,
                    "width": 53,
                    "top": 152,
                    "left": 339,
                    "class-id": "1",
                    "label-category-attributes": {
                        "occluded": "no",
                        "size": "medium"
                    }
                },
                {
                    "height": 24,
                    "width": 37,
                    "top": 148,
                    "left": 183,
                    "class-id": "0",
                    "label-category-attributes": {
                        "occluded": "no",
                    }
                }
            ],
            "frame-no": "0",
            "frame": "frame_0000.jpeg"
        },
        {
            "annotations": [
                {
                    "height": 41,
                    "width": 53,
                    "top": 152,
                    "left": 341,
                    "class-id": "0",
                    "label-category-attributes": {}
                },
                {
                    "height": 24,
                    "width": 37,
                    "top": 141,
                    "left": 177,
                    "class-id": "0",
                    "label-category-attributes": {
                        "occluded": "no",
                    }
                }
            ],
            "frame-no": "1",
            "frame": "frame_0001.jpeg"
        }
    ]
}
```

## Video Frame Object Tracking Output<a name="sms-output-video-object-tracking"></a>

The following is the output manifest file from a video frame object tracking labeling job\. The *red, italicized text* in the examples below depends on labeling job specifications and output data\.

In addition to the standard elements, the metadata includes a class map that lists each class that has at least one label in the sequence of frames\. The metadata also includes `job-name` which is the name you assigned to the labeling job\. For adjustment tasks, If one or more bounding boxes were modified, there is an `adjustment-status` parameter in the metadata for audit workflows that is set to `adjusted`\. 

```
{
    "source-ref": "s3://DOC-EXAMPLE-BUCKET/example-path/input-manifest.json",
    "CarObjectTracking-ref": "s3://AWSDOC-EXAMPLE-BUCKET/output/labeling-job-name/annotations/consolidated-annotation/output/0/SeqLabel.json",
    "CarObjectTracking-ref-metadata": {
        "class-map": {
            "0": "car"
            "1": "bus"
        },
        "job-name": "labeling-job/labeling-job-name",
        "human-annotated": "yes",
        "creation-date": "2020-05-15T08:01:16+0000",
        "type": "groundtruth/video-object-tracking"
        }
 }
```

Ground Truth creates one output sequence file for each sequence of video frames that was labeled\. Each output sequence file contains the following: 
+ All annotations for all frames in a sequence in the `tracking-annotations` list of JSON objects\. 
+ For each frame that was annotated by a worker, the frame file name \(`frame`\), number \(`frame-no`\) and an `annotations` list of JSON objects\. 

  Each JSON object contains information about a single bounding box and associated label:
  + Box dimensions: `height` and `width` 
  + Box pixel location: `top` and `left`
  + Values of any `label-category-attributes` that were specified for that label\. 
  + The `class-id` of the box\. Use the `class-map` in the output manifest file to see which label category this ID maps to\. 
  + An `object-id` which identifies an instance of a label\. This ID will be the same across frames if a worker identified the same instance of an object in multiple frames\. For example, if a car appeared in multiple frames, all bounding boxes uses to identify that car would have the same `object-id`\.
  + The `object-name` which is the instance ID of that annotation\. 

The following is an example of a `SeqLabel.json` file:

```
{
    "tracking-annotations": [
        {
            "annotations": [
                {
                    "height": 36,
                    "width": 46,
                    "top": 178,
                    "left": 315,
                    "class-id": "0",
                    "label-category-attributes": {
                        "occluded": "no"
                    },
                    "object-id": "480dc450-c0ca-11ea-961f-a9b1c5c97972",
                    "object-name": "car:1"
                }
            ],
            "frame-no": "0",
            "frame": "frame_0001.jpeg"
        },
        {
            "annotations": [
                {
                    "height": 30,
                    "width": 47,
                    "top": 163,
                    "left": 344,
                    "class-id": "1",
                    "label-category-attributes": {
                        "occluded": "no",
                        "size": "medium"
                    },
                    "object-id": "98f2b0b0-c0ca-11ea-961f-a9b1c5c97972",
                    "object-name": "bus:1"
                },
                {
                    "height": 28,
                    "width": 33,
                    "top": 150,
                    "left": 192,
                    "class-id": "0",
                    "label-category-attributes": {
                        "occluded": "partially"
                    },
                    "object-id": "480dc450-c0ca-11ea-961f-a9b1c5c97972",
                    "object-name": "car:1"
                }
            ],
            "frame-no": "1",
            "frame": "frame_0002.jpeg"
        }
    ]
}
```

## 3D Point Cloud Semantic Segmentation Output<a name="sms-output-point-cloud-segmentation"></a>

The following is the output manifest file from a 3D point cloud semantic segmentation labeling job\. 

In addition to the standard elements, the metadata for the label includes a color map that defines which color is used to label the image, the class name associated with the color, and the confidence score for each color\. Additionally, there is an `adjustment-status` parameter in the metadata for audit workflows that is set to `adjusted` if the color mask is modified\. 

The `your-label-attribute-ref` parameter contains the location of a compressed file with a \.zlib extension\. When you uncompress this file, it contains an array that indicates the indexes of each annotated point in the input point cloud and the value of the array gives the class of the point based on the semantic color map found in metadata\. 

The red, italicized text in the examples below depends on labeling job specifications and output data\. 

```
{
   "source-ref": "s3://AWSDOC-EXAMPLE-BUCKET/examplefolder/frame1.bin",
   "source-ref-metadata":{
      "format": "binary/xyzi",
      "unix-timestamp": 1566861644.759115,
      "ego-vehicle-pose":{...}, 
      "prefix": "s3://AWSDOC-EXAMPLE-BUCKET/lidar_singleframe_dataset/prefix",
      "images": [{...}] 
    },
    "lidar-ss-label-attribute-ref": "s3://your-output-bucket/labeling-job-name/annotations/consolidated-annotation/output/dataset-object-id/filename.zlib",
    "lidar-ss-label-attribute-ref-metadata": { 
        'color-map': {
            "0": {
                "class-name": "Background",
                "hex-color": "#ffffff",
                "confidence": 0.00
            },
            "1": {
                "class-name": "Car",
                "hex-color": "#2ca02c",
                "confidence": 0.00
            },
            "2": {
                "class-name": "Pedestrian",
                "hex-color": "#1f77b4",
                "confidence": 0.00
            },
            "3": {
                "class-name": "Tree",
                "hex-color": "#ff7f0e",
                "confidence": 0.00
            }
        },
        'type': 'groundtruth/lidar_single_frame_semantic_segmentation', 
        'human-annotated': 'yes',
        'creation-date': '2019-11-12T01:18:14.271944',
        'job-name': 'labeling-job-name',
        //only present for adjustment audit workflow
        "adjustment-status": "adjusted" // adjusted only if the label was adjusted
    }
}
```

## 3D Point Cloud Object Detection Output<a name="sms-output-point-cloud-object-detection"></a>

The following is sample output from a 3D point cloud objected detection job\. For this task type, the data about 3D cuboids is returned in the `3d-bounding-box` parameter, in a list named `annotations`\. In this list, each 3D cuboid is described using the following information\. If one or more cuboids were modified, there is an `adjustment-status` parameter in the metadata for audit workflows that is set to `adjusted`\. 
+ Each class, or label category, that you specify in your input manifest is associated with a `class-id`\. Use the `class-map` to identify the class associated with each class ID\.
+ These classes are used to give each 3D cuboid an `object-name` in the format `<class>:<integer>` where `integer` is a unique number to identify that cuboid in the frame\. 
+ `center-x`, `center-y`, and `center-z` are used to describe the center of the cuboid in the world coordinate system\. 
+ `length`, `width`, and `height` are used to describe the dimensions of the cuboid\. 
+ `roll`, `pitch`, and `yaw` are used to describe the orientation \(heading\) of the cuboid\. 
+ If you included label attributes in your input manifest file for a given class, a `label-category-attributes` parameter is included for all cuboids for which workers selected label attributes\. 

The *red, italicized text* in the examples below depends on labeling job specifications and output data\. The ellipses \(*\.\.\.*\) denote a continuation of that list, where additional objects with the same format as the proceeding object can appear\.

```
{
   "source-ref": "s3://AWSDOC-EXAMPLE-BUCKET/examplefolder/frame1.txt",
   "source-ref-metadata":{
      "format": "text/xyzi",
      "unix-timestamp": 1566861644.759115, 
      "prefix": "s3://AWSDOC-EXAMPLE-BUCKET/lidar_singleframe_dataset/prefix",
      "ego-vehicle-pose": {
            "heading": {
                "qx": -0.02111296123795955,
                "qy": -0.006495469416730261,
                "qz": -0.008024565904865688,
                "qw": 0.9997181192298087
            },
            "position": {
                "x": -2.7161461413869947,
                "y": 116.25822288149078,
                "z": 1.8348751887989483
            }
       },
       "images": [
            {
                "fx": 847.7962624528487,
                "fy": 850.0340893791985,
                "cx": 576.2129134707038,
                "cy": 317.2423573573745,
                "k1": 0,
                "k2": 0,
                "k3": 0,
                "k4": 0,
                "p1": 0,
                "p2": 0,
                "skew": 0,
                "unix-timestamp": 1566861644.759115,
                "image-path": "images/frame_0_camera_0.jpg",
                "position": {
                    "x": -2.2722515189268138,
                    "y": 116.86003310568965,
                    "z": 1.454614668542299
                },
                "heading": {
                    "qx": 0.7594754093069037,
                    "qy": 0.02181790885672969,
                    "qz": -0.02461725233103356,
                    "qw": -0.6496916273040025
                },
                "camera_model": "pinhole"
            }
        ]
    },
   "3d-bounding-box": 
    {
       "annotations": [
            {
                "label-category-attributes": {
                    "Occlusion": "Partial",
                    "Type": "Sedan"
                },
                "object-name": "Car:1",
                "class-id": 0,
                "center-x": -2.616382013657516,
                "center-y": 125.04149850484193,
                "center-z": 0.311272296465834,
                "length": 2.993000265181146,
                "width": 1.8355260519692056,
                "height": 1.3233490884304047,
                "roll": 0,
                "pitch": 0,
                "yaw": 1.6479308313703527
            },
            {
                "label-category-attributes": {
                    "Occlusion": "Partial",
                    "Type": "Sedan"
                },
                "object-name": "Car:2",
                "class-id": 0,
                "center-x": -5.188984560617168,
                "center-y": 99.7954483288783,
                "center-z": 0.2226435567445657,
                "length": 4,
                "width": 2,
                "height": 2,
                "roll": 0,
                "pitch": 0,
                "yaw": 1.6243170732068055
            }
        ]
    },
    "3d-bounding-box-metadata":
    {
        "objects": [], 
        "class_map": 
        {
            "0": "Car",
        },
        "type": "groundtruth/lidar_object_detection",
        "human-annotated": "yes", 
        "creation-date": "2018-10-18T22:18:13.527256",
        "job-name": "identify-3d-objects",
        "adjustment-status": "adjusted"
    }
}
```

## 3D Point Cloud Object Tracking Output<a name="sms-output-point-cloud-object-tracking"></a>

The following is an example of an output manifest file from a 3D point cloud object tracking labeling job\. The *red, italicized text* in the examples below depends on labeling job specifications and output data\. The ellipses \(*\.\.\.*\) denote a continuation of that list, where additional objects with the same format as the proceeding object can appear\.

In addition to the standard elements, the metadata includes a class map that lists each class that has at least one label in the sequence\. If one or more cuboids were modified, there is an `adjustment-status` parameter in the metadata for audit workflows that is set to `adjusted`\. 

```
{
   "source-ref": "s3://AWSDOC-EXAMPLE-BUCKET/myfolder/seq1.json",
    "lidar-label-attribute-ref": "s3://<CustomerOutputLocation>/<labelingJobName>/annotations/consolidated-annotation/output/<datasetObjectId>/SeqLabel.json",
    "lidar-label-attribute-ref-metadata": { 
        "objects": 
        [
            {   
                "frame-no": 300,
                "confidence": []
            },
            {
                "frame-no": 301,
                "confidence": []
            },
            ...
        ],    
        'class-map': {'0': 'Car', '1': 'Person'}, 
        'type': 'groundtruth/lidar_object_tracking', 
        'human-annotated': 'yes',
        'creation-date': '2019-11-12T01:18:14.271944',
        'job-name': 'identify-3d-objects',
        "adjustment-status": "adjusted" 
    }
}
```

In the above example, the cuboid data for each frame in `seq1.json` is in `SeqLabel.json` in the Amazon S3 location, `s3://<customerOutputLocation>/<labelingJobName>/annotations/consolidated-annotation/output/<datasetObjectId>/SeqLabel.json`\. The following is an example of this label sequence file\.

For each frame in the sequence, you see a list of 3D cubiods that were drawn for that frame\. Each frame includes the following information: 
+ An `object-name` in the format `<class>:<integer>` where `class` identifies the label category and `integer` is a unique ID across the the dataset\.
+ When workers draw a cuboid, it is associated with a unique `object-id` which is associated with all cuboids that identify the same object across multiple frames\.
+ Each class, or label category, that you specified in your input manifest is associated with a `class-id`\. Use the `class-map` to identify the class associated with each class ID\.
+ `center-x`, `center-y`, and `center-z` describe the center of the cuboid in the world coordinate system\. 
+ `length`, `width`, and `height` describe the dimensions of the cuboid\. 
+ `roll`, `pitch`, and `yaw` describe the orientation \(heading\) of the cuboid\. 
+ If you included label attributes in your input manifest file for a given class, a `label-category-attributes` parameter is included for all cuboids for which workers selected label attributes\. 

```
{
    "tracking-annotations": [
        {
            "frame-number": 0,
            "frame-name": "0.txt.pcd",
            "annotations": [
                {
                    "label-category-attributes": {},
                    "object-name": "Car:4",
                    "class-id": 0,
                    "center-x": -2.2906369208300674,
                    "center-y": 103.73924823843463,
                    "center-z": 0.37634114027023313,
                    "length": 4,
                    "width": 2,
                    "height": 2,
                    "roll": 0,
                    "pitch": 0,
                    "yaw": 1.5827222214406014,
                    "object-id": "ae5dc770-a782-11ea-b57d-67c51a0561a1"
                },
                {
                    "label-category-attributes": {
                        "Occlusion": "Partial",
                        "Type": "Sedan"
                    },
                    "object-name": "Car:1",
                    "class-id": 0,
                    "center-x": -2.6451293634707413,
                    "center-y": 124.9534455706848,
                    "center-z": 0.5020834081743839,
                    "length": 4,
                    "width": 2,
                    "height": 2.080488827301309,
                    "roll": 0,
                    "pitch": 0,
                    "yaw": -1.5963335581398077,
                    "object-id": "06efb020-a782-11ea-b57d-67c51a0561a1"
                },
                {
                    "label-category-attributes": {
                        "Occlusion": "Partial",
                        "Type": "Sedan"
                    },
                    "object-name": "Car:2",
                    "class-id": 0,
                    "center-x": -5.205611313118477,
                    "center-y": 99.91731932137061,
                    "center-z": 0.22917217081212138,
                    "length": 3.8747142207671956,
                    "width": 1.9999999999999918,
                    "height": 2,
                    "roll": 0,
                    "pitch": 0,
                    "yaw": 1.5672228760316775,
                    "object-id": "26fad020-a782-11ea-b57d-67c51a0561a1"
                }
            ]
        },
        {
            "frame-number": 1,
            "frame-name": "1.txt.pcd",
            "annotations": [
                {
                    "label-category-attributes": {},
                    "object-name": "Car:4",
                    "class-id": 0,
                    "center-x": -2.2906369208300674,
                    "center-y": 103.73924823843463,
                    "center-z": 0.37634114027023313,
                    "length": 4,
                    "width": 2,
                    "height": 2,
                    "roll": 0,
                    "pitch": 0,
                    "yaw": 1.5827222214406014,
                    "object-id": "ae5dc770-a782-11ea-b57d-67c51a0561a1"
                },
                {
                    "label-category-attributes": {
                        "Occlusion": "Partial",
                        "Type": "Sedan"
                    },
                    "object-name": "Car:1",
                    "class-id": 0,
                    "center-x": -2.6451293634707413,
                    "center-y": 124.9534455706848,
                    "center-z": 0.5020834081743839,
                    "length": 4,
                    "width": 2,
                    "height": 2.080488827301309,
                    "roll": 0,
                    "pitch": 0,
                    "yaw": -1.5963335581398077,
                    "object-id": "06efb020-a782-11ea-b57d-67c51a0561a1"
                },
                {
                    "label-category-attributes": {
                        "Occlusion": "Partial",
                        "Type": "Sedan"
                    },
                    "object-name": "Car:2",
                    "class-id": 0,
                    "center-x": -5.221311072916759,
                    "center-y": 100.4639841045424,
                    "center-z": 0.22917217081212138,
                    "length": 3.8747142207671956,
                    "width": 1.9999999999999918,
                    "height": 2,
                    "roll": 0,
                    "pitch": 0,
                    "yaw": 1.5672228760316775,
                    "object-id": "26fad020-a782-11ea-b57d-67c51a0561a1"
                }
            ]
        }       
    ]
}
```