# Output Data<a name="sms-data-output"></a>

The output from a labeling job is placed in the location that you specified in the console or in the call to the [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\.

Each line in the output data file is identical to the manifest file with the addition of an attribute and value for the label assigned to the input object\. The attribute name for the value is defined in the console or in the call to the `CreateLabelingJob` operation\. You can't use `-metadata` in the label attribute name\. If you are running a semantic segmentation job, the label attribute must end with `-ref`\. For any other type of job, the attribute name can't end with `-ref`\.

The output of the labeling job is the value of the key/value pair with the label\. The label and the value overwrites any existing JSON data in the input file with the new value\.

For example, the following is the output from an image classification labeling job where the input data files were stored in an Amazon S3 bucket and the label attribute name was defined as "sport"\. In this example the JSON object is formatted for readability, in the actual output file the JSON object is on a single line\. For more information about the data format, see [JSON Lines](http://jsonlines.org/)\.

```
{
    "source-ref": "S3 bucket location",
    "sport":0,
    "sport-metadata":
    {
        "class-name": "football",
        "confidence": 0.8,
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

The output data file is written to the output location periodically while the job is in progress\. These intermediate files contain one line for each line in the manifest file\. If an object is labeled, the label is included, if the object has not been labeled it is written to the intermediate output file identically to the manifest file\.

## Output Directories<a name="sms-output-directories"></a>

Ground Truth creates several directories in your Amazon S3 output path\. These directories contain the results of your labeling job and other artifacts of the job\. The top\-level directory for a labeling job is given the same name as your labeling job, the output directories are placed beneath it\. For example, if you named your labeling job **find\-people** you output would be in the following directories:

```
s3://bucket/find-people/activelearning
s3://bucket/find-people/annotations
s3://bucket/find-people/inference
s3://bucket/find-people/manifests
s3://bucket/find-people/training
```

Each directories contain the following output:

### Active Learning Directory<a name="sms-output-activelearning"></a>

The `activelearning` directory is only present when you are using automated data labeling\. It contains the input and output validation set for automated data labeling, and the input and output folder for automatically labeled data\.

### Annotations Directory<a name="sms-directories-annotations"></a>

The `annotations` directory contains all of the annotations made by the workforce\. These are the responses from individual workers that have not been consolidated into a single label for the data object\. 

There are three subdirectories in the `annotations` directory\. 

The first, `worker-response` contains the responses from individual workers\. This contains a subdirectory for each iteration, which in turn contains a subdirectory for each data object in that iteration\. The annotation for each data object is stored in a timestamped \.json file\. There may be more than one annotation for each data object in this directory, depending on how many workers you want to annotate each object\.

The second, `consolidated-annotation` contains information required to consolidate the annotations in the current batch into labels for your data objects\.

The third, `intermediate`, contains the output manifest for the current batch with any completed labels\. This file is updated as the label for each data object is completed\.

### Inference Directory<a name="sms-directories-inference"></a>

The `inference` directory is only present when you are using automated data labeling\. This directory contains the input and output files for the Amazon SageMaker batch transform used while labeling data objects\.

### Manifest Directory<a name="sms-directories-manifest"></a>

The `manifest` directory contains the output manifest from your labeling job\. There is one subdirectory in the manifest directory, `output`\. The `output` directory contains the output manifest file for your labeling job\. The file is named `output.manifest`\.

### Training Directory<a name="sms-directories-training"></a>

The `training` directory is only present when you are using automated data labeling\. This directory contains the input and output files used to train the automated data labeling model\.

## Confidence Score<a name="sms-output-confidence"></a>

Ground Truth calculates a confidence score for each label\. A *confidence score* is a number between 0 and 1 that indicates how confident Ground Truth is in the label\. You can use the confidence score to compare labeled data objects to each other, and to identify the least or most confident labels\.

You should not interpret the value of the confidence scores as an absolute value, or compare them across labeling jobs\. For example, if all of the confidence scores are between 0\.98 and 0\.998, you should only compare the data objects with each other and not rely on the high confidence scores\. 

You should not compare the confidence scores of human\-labeled data objects and auto\-labeled data objects\. The confidence scores for humans are calculated using the annotation consolidation function for the task, the confidence scores for automated labeling are calculated using a model that incorporates object features\. The two models generally have different scales and average confidence\.

For a bounding box labeling job, Ground Truth calculates a confidence score per box\. You can compare confidence scores within one image or across images for the same labeling type \(human or auto\)\. You can't compare confidence scores across labeling jobs\.

## Output Metadata<a name="sms-output-metadata"></a>

The output from each job contains metadata about the label assigned to data objects\. These elements are the same for all jobs with minor variations\. The following example shows the metadata elements\.

```
    "confidence": 0.93,
    "type": "groundtruth/image-classification",
    "job-name": "identify-animal-species",
    "human-annotated": "yes",
    "creation-date": "2018-10-18T22:18:13.527256"
```

The elements have the following meaning:
+ `confidence` – The confidence that Ground Truth has that the label is correct\. For more information, see [Confidence Score](#sms-output-confidence)\.
+ `type` – The type of classification job\. For job types, see [Built in Task Types](sms-task-types.md)\. 
+ `job-name` – The name assigned to the job when it was created\.
+ `human-annotated` – Indicates whether the data object was labeled by a human or by automated data labeling\. For more information, see [Automate Data Labeling](sms-automated-labeling.md)\.
+ creation\-date`` – The date and time that the label was created\.

## Classification Job Output<a name="sms-output-class"></a>

The following are sample outputs \(output manifest files\) from an image classification job and a text classification job\. They includes the label that Ground Truth assigned to the data object, the value for the label, and metadata that describes the label\.

In addition to the standard metadata elements, the metadata for a classification job includes the text value of the label's class\. For more information, see [Image Classification Algorithm](image-classification.md)\.

```
{
    "source-ref":"S3 bucket location",
    "species":"0",
    "species-metadata":
    {
        "class-name": "dog",
        "confidence": 0.93,
        "type": "groundtruth/image-classification",
        "job-name": "identify-animal-species",
        "human-annotated": "yes",
        "creation-date": "2018-10-18T22:18:13.527256"
    }
}
```

```
{
    "source":"a bad day",
    "mood":"1",
    "mood-metadata":
    {
        "class-name": "sad",
        "confidence": 0.8,
        "type": "groundtruth/text-classification",
        "job-name": "label-mood",
        "human-annotated": "yes",
        "creation-date": "2018-10-18T22:18:13.527256"
    }
}
```

## Multi\-label Classification Job Output<a name="sms-output-multi-label-classification"></a>

The following are example output manifest files from a multi\-label image classification job and a multi\-label text classification job\. They include the labels that Ground Truth assigned to the data object \(for example the image or piece of text\) and metadata that describes the labels the the worker saw when completing the labeling task\. 

The label attribute name parameter \(for example, `image-label-attribute-name`\) contains an array of all of the labels that were selected by at least one of the workers who completed this task\. This array contains integer keys \(for example, `[1,0,8]`\) that correspond to the labels found in `class-map`\. In the multi\-label image classification example, `bicycle`, `person`, and `clothing` were selected by at least one of the workers who completed the labeling task for the image, `exampleimage.jpg`\.

The `confidence-map` shows the confidence\-score that Ground Truth assigned to each label that was selected by a worker\. To learn more about Ground Truth confidence scores, see [Confidence Score](#sms-output-confidence)\.

The following is an example of a multi\-label image classification output manifest file\. 

```
{
    "source-ref": "awsexamplebucket/exampleimage.jpg",
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

The following is an example of a multi\-label text classification output manifest file\. In this example, `approving`, `sad` and `critical` were selected by at least one of the workers who completed the labeling task for the object `exampletext.txt` found in `awsexamplebucket1`\.

```
{
    "source-ref": "awsexamplebucket1/exampletext.txt",
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

The output \(output manifest file\) of a bounding box verification job looks much different than the output of a bounding box annotation job\. That's because the workers having a different type of task\. They're not labeling objects, but evaluating the accuracy of prior labeling, making a judgment, and then providing that judgment and perhaps some comments\.

If you are having human workers verify or adjust prior bounding box labels, the output of a verification job would look like the following JSON\.

```
{
    "source-ref":"S3 bucket location",
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

The following is the output manifest file from a semantic segmentation labeling job\. The value of the label for this job is a reference to a PNG file in an S3 bucket\.

In addition to the standard elements, the metadata for the label includes a color map that defines which color was used to label the image, the class name associated with the color, and the confidence score for each color\. For more information, see [Semantic Segmentation Algorithm](semantic-segmentation.md)\.

```
{
    "source-ref": "S3 bucket location",
    "city-streets-ref": "S3 bucket location",
    "city-streets-metadata": {
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
    "source-ref": "S3 bucket location",
    "city-streets-ref": "S3 bucket location",
    "city-streets-metadata": {
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
     "adjusted-city-streets-ref": "S3 bucket location",
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

After you create an augmented manifest file, you can use it in a training job\. See [object\_detection\_augmented\_manifest\_training\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/ground_truth_labeling_jobs/object_detection_augmented_manifest_training/object_detection_augmented_manifest_training.ipynb) for a demonstration of using of an "augmented manifest" to train an object detection machine learning model with AWS SageMaker\. For more information, see [Provide Dataset Metadata to Training Jobs with an Augmented Manifest File](augmented-manifest.md)\.