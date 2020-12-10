# Create a Labeling Category Configuration File with Label Category Attributes<a name="sms-label-cat-config-attributes"></a>

When you create a 3D point cloud or video frame labeling job using the Amazon SageMaker API operation `CreateLabelingJob`, you use a label category configuration file to specify your labels and worker instructions\. Optionally, you can also provide the following in your label category attribute file:
+ You can provide *label category attributes* for video frame and 3D point cloud object tracking and object detection task types\. Workers can use one or more attributes to give more information about an object\. For example, you may want to use the attribute *occluded* to have workers identify when an object is partially obstructed\. You can either specify a label category attribute for a single label using the `categoryAttributes` parameter, or for all labels using the `categoryGlobalAttributes` parameter\. 
+ You can provide *frame attributes* for video frame and 3D point cloud object tracking and object detection task types using `frameAttributes`\. When you create a frame attribute, it appears on each frame or point cloud in the worker task\. In video frame labeling jobs, these are attributes that workers assign to an entire video frame\. For 3D point cloud labeling jobs, these attributes are applied to a single point cloud\. Use frame attributes to have workers provide more information about the scene in a specific frame or point cloud\.
+ For video frame labeling jobs, you use the label category configuration file to specify the task type \(bounding box, polyline, polygon, or keypoint\) sent to workers\. 

**Important**  
You should only provide a label attribute name in `auditLabelAttributeName` if you are running an audit job to verify or adjust labels\. Use this parameter to input the [LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) used in the labeling job that generated the annotations you want your worker to adjust\. When you create a labeling job in the console, if you did not specify a label attribute name, the **Name** of your job is used as the LabelAttributeName\.

**Topics**
+ [Label Category Configuration File Schema](#sms-label-cat-config-attributes-schema)
+ [Example: Label Category Configuration File for 3D Point Cloud Labeling Jobs](#sms-label-cat-config-attributes-3d-pc)
+ [Example: Label Category Configuration File for Video Frame Labeling Jobs](#sms-label-cat-config-attributes-vid-frame)
+ [Creating Worker Instructions](#sms-point-cloud-worker-instructions-create)

## Label Category Configuration File Schema<a name="sms-label-cat-config-attributes-schema"></a>

The following table lists elements you can and must include in your label category configuration file\.

**Note**  
The parameter `annotationType` is only supported for video frame labeling jobs\. 


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
| frameAttributes |  No  |  A list of JSON objects\. **Required Parameters in each JSON Object:** `name` \(String\): The name of the attribute displayed to workers\. Each frame attribute name in your label category configuration file must be unique\.  `type` \(String\): The type of attribute\. Valid values: `"string"` or `"number"`\. `description` \(String\): A description of the attribute\. Use this field to give workers more information\.  `minimum` and `maximum`: **Required** if `type` is `"number"`\. Use these parameters to specify minimum and maximum \(inclusive\) values workers can enter for numeric frame attributes\.  **Optional Parameters in each JSON Object:** `enum` \(List of strings\): `type` must be `"string"`\. Choices that the workers can choose from\. Workers can assign one value from `enum` to the attribute\. Example value: `["foo", "buzz", "bar"`\]  | Use this parameter to create a frame attribute that is applied to all frames or 3D point clouds in your labeling job\.Use the `description` parameter to provide more information about your frame attribute\. See the following table for more information\.  | 
| categoryGlobalAttributes |  No  |  A list of JSON objects\. **Required Parameters in each JSON Object:** `name` \(String\): The name of the attribute displayed to workers\. Each label category attribute name in your label category configuration file must be unique\. Global label category attributes and label specific label category attributes cannot have the same name\.  `type` \(String\): The type of attribute\. Valid values: `"string"` or `"number"`\. `minimum` and `maximum`: **Required** if `type` is `"number"`\. Use these parameters to specify minimum and maximum \(inclusive\) values workers can enter for numeric frame attributes\.  **Optional Parameters in each JSON Object:** `description` \(String\): A description of the attribute\. Use this field to give workers more information\.  `enum` \(List of strings\): `type` must be `"string"`\. Choices that the workers can choose from\. Workers can assign one value from `enum` to the attribute\. Example value: `["foo", "buzz", "bar"`\]  | Use this parameter to create a label category attribute that is applied to all labels you specify in `labels`\. Use the `description` parameter to provide more information about your global label category attribute\. See the following table for more information\.  | 
| labels |  Yes  |  A list of up to 30 JSON objects **Required Parameters in each JSON Object:** `label` **Optional Parameters in each JSON Object:** `categoryAttributes`  |  Use this parameter to specify your labels, or classes\. Add one `label` for each class\.  To add a label category attribute to a label, add `categoryAttributes` to that label\.  | 
| annotationType \(only supported for video frame labeling jobs\)  |  No   |  String **Accepted Parameters:** `BoundingBox`, `Polyline`, `Polygon`, `Keypoint` **Default:** `BoundingBox`  |  Use this to specify the task type for your video frame labeling jobs\. For example, for a polygon video frame object detection task, choose `Polygon`\.  If you do not specify an `annotationType` when you create a video frame labeling job, Ground Truth will use `BoundingBox` by default\.   | 
| categoryAttributes |  No  |  A list of JSON objects\. **Required Parameters in each JSON Object:** `name` \(String\): The name of the attribute displayed to workers\. Each label category attribute name in your label category configuration file must be unique\.  `type` \(String\): The type of attribute\. Valid values: `"string"` or `"number"`\. `minimum` and `maximum`: **Required** if `type` is `"number"`\. Use these parameters to specify minimum and maximum \(inclusive\) values workers can enter for numeric frame attributes\.  **Optional Parameters in each JSON Object:** `description` \(String\): A description of the attribute\. Use this field to give workers more information\.  `enum` \(List of strings\): Choices that the workers can choose from\. Workers can assign one value from `enum` to the attribute\. Example value: `["foo", "buzz", "bar"`\]  | Use this parameter to add label category attributes to a specific labels you specify in `label` in the same JSON object under `labels`\. Use the `description` parameter to provide more information about your label category attribute\. See the following table for more information\.  | 
| instructions |  No  | A JSON objectRequired Parameters in each JSON Object:`"shortInstruction"`, `"fullInstruction"` |  Use this parameter to add worker instructions to help your workers complete their tasks\. For more information about worker instructions, see [Worker Instructions](sms-point-cloud-general-information.md#sms-point-cloud-worker-instructions-general)\.  Short instructions must be under 255 characters and long instruction must be under 2,048 characters\.  For more information, see [Creating Worker Instructions](#sms-point-cloud-worker-instructions-create)\.  | 
| auditLabelAttributeName |  For Adjustment Task Types  |  String  |  Enter the [LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) used in the labeling job you want to adjust annotations of\.  Only use this parameter if you are creating an adjustment job for video frame and 3D point cloud object detection, object tracking, or 3D point cloud semantic segmentation\.   | 

The following table describes the parameters that you can and must use to create a frame attributes in `frameAttributes` and label category attribute in the `categoryGlobalAttributes` and `categoryAttributes` parameters\.


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
| name |  Yes  |  String  |  Use this parameter to assign a name to your label category attribute\. This is the name that workers see for this attribute\.   | 
| type |  Yes  |  String **Supported Values**: `"string"`, `"number"`  |  Use this parameter to define the label category attribute type\.  If you specify `"string"` for `type` and provide an `enum` value for this attribute, workers will be able to choose from one of the choices you provide\.  If you specify `"string"` for `type` and do not provide an `enum` value, workers can enter free form text\.  If you specify `number` for `type`, worker can enter a number between the `minimum` and `maximum` numbers you specify\.   | 
| enum |  Yes  |  List of strings  |  Use this parameter to define options that workers can choose from for this label category attribute\. Workers can choose one value specified in `enum`\.  You must specify `"string"` for `type` if you use an `enum` list\.  | 
| description |  `frameAttributes`: Yes `categoryAttributes` or `categoryGlobalAttributes`: No  |  String  |   Use this parameter to add a description of the label category attribute\.   | 
| minimum and maximum | If attribute type is "number": Yes | Integers |  Use these parameters to specify minimum and maximum \(inclusive\) values workers can enter for numeric frame attributes\. You must specify `"number"` for `type` to use `minimum` and `maximum`\.  | 

### Label and label category attribute quotas<a name="sms-point-cloud-label-cat-limits"></a>

You can specify up to 10 label category attributes per class\. This 10\-attribute quotas includes global label category attributes\. For example, if you create four global label category attributes, and then assign three label category attributes to label `X`, that label will have 4\+3=7 label category attributes in total\. For all label category and label category attribute limits, refer to the following table\.


****  

|  Type  |  Min  |  Max  | 
| --- | --- | --- | 
|  Labels \(`Labels`\)  |  1  |  30  | 
|  Label name character quota  |  1  |  16  | 
|  Label category attributes per label \(sum of `categoryAttributes` and `categoryGlobalAttributes`\)  |  0  |  10  | 
|  Free form text entry label category attributes per label \(sum of `categoryAttributes` and `categoryGlobalAttributes`\)\.   | 0 | 5 | 
|  Frame attributes  |  0  |  10  | 
|  Free form text entry attributes in `frameAttributes`\.  | 0 | 5 | 
|  Attribute name character quota \(`name`\)  |  1  |  16  | 
|  Attribute description character quota \(`description`\)  |  0  |  128  | 
|  Attribute type characters quota \(`type`\)  |  1  |  16  | 
|  Allowed values in the `enum` list for a `string` attribute  | 1 | 10 | 
|  Character quota for a value in `enum` list  | 1 | 16 | 
| Maximum characters in free form text response for free form text frameAttributes | 0 | 1000 | 
| Maximum characters in free form text response for free form text categoryAttributes and categoryGlobalAttributes | 0 | 80 | 

## Example: Label Category Configuration File for 3D Point Cloud Labeling Jobs<a name="sms-label-cat-config-attributes-3d-pc"></a>

The following is an example of a label category configuration file that includes label category attributes that you may use for a 3D point cloud labeling job\. See the quotas for these parameters in [Label and label category attribute quotas](#sms-point-cloud-label-cat-limits)\. This example includes a two frame attributes, which will be added to all point clouds submitted to the labeling job\. The `Car` label will include four label category attributes—`X`, `Y`, `Z`, and the global attribute, `W`\. 

```
{
    "documentVersion": "2020-03-01",
    "frameAttributes": [
        {
            "name":"count players",
            "description":"How many players to you see in the scene?",
            "type":"number"
        },
        {
            "name":"select one",
            "description":"describe the scene",
            "type":"string",
            "enum":["clear","blurry"]
        },   
    ],
    "categoryGlobalAttributes": [
        {
            "name":"W",
            "description":"label-attributes-for-all-labels",
            "type":"string",
            "enum": ["foo", "buzz", "biz"]
        }
    ],
    "labels": [
        {
            "label": "Car",
            "categoryAttributes": [
                {
                    "name":"X",
                    "description":"enter a number",
                    "type":"number",
                },
                {
                    "name":"Y",
                    "description":"select an option",
                    "type":"string",
                    "enum":["y1", "y2"]
                },
                {
                    "name":"Z",
                    "description":"submit a free-form response",
                    "type":"string",
                }
            ]
        },
        {
            "label": "Pedestrian",
            "categoryAttributes": [...]
        }
    ],
    "instructions": {"shortInstruction":"Draw a tight Cuboid", "fullInstruction":"<html markup>"},
    // include auditLabelAttributeName for label adjustment jobs
    "auditLabelAttributeName": "myPrevJobLabelAttributeName"
}
```

Label category attributes are not supported for 3D point cloud semantic segmentation task types\. Frame attributes are supported\. If you provide label category attributes for a semantic segmentation labeling job, they will be ignored\. Your label category configure file for a 3D point cloud semantic segmentation labeling job may look similar to the following:

```
{
    "documentVersion": "2020-03-01",
    "frameAttributes": [
        {
            "name":"count players",
            "description":"How many players to you see in the scene?",
            "type":"number"
        },
        {
            "name":"select one",
            "description":"describe the scene",
            "type":"string",
            "enum":["clear","blurry"]
        },   
    ],
    "labels": [
        {
            "label": "Car",
        },
        {
            "label": "Pedestrian",
        },
        {
            "label": "Cyclist",
        }
    ],
    "instructions": {"shortInstruction":"Select the appropriate label and paint all objects in the point cloud that it applies to the same color", "fullInstruction":"<html markup>"},
    // include auditLabelAttributeName for label adjustment jobs
    "auditLabelAttributeName": "myPrevJobLabelAttributeName"
}
```

## Example: Label Category Configuration File for Video Frame Labeling Jobs<a name="sms-label-cat-config-attributes-vid-frame"></a>

The annotation tools available to your worker and task type used depends on the value you specify for `annotationType`\. For example, if you want workers to use key points to track changes in the pose of specific objects across multiple frames, you would specify `Keypoint` for the `annotationType`\. If you do not specify an annotation type, `BoundingBox` will be used by default\. 

The following is an example of a video frame keypoint label category configuration file with label category attributes\. This example includes two frame attributes, which will be added to all frames submitted to the labeling job\. The `Car` label will include four label category attributes—`X`, `Y`, `Z`, and the global attribute, `W`\. 

```
{
    "documentVersion": "2020-03-01",
    "frameAttributes": [
        {
            "name":"count players",
            "description":"How many players to you see in the scene?",
            "type":"number"
        },
        {
            "name":"select one",
            "description":"describe the scene",
            "type":"string",
            "enum":["clear","blurry"]
        },   
    ],
    "categoryGlobalAttributes": [
        {
            "name":"W",
            "description":"label-attributes-for-all-labels",
            "type":"string",
            "enum": ["foo", "buz", "buz2"]
        }
    ],
    "labels": [
        {
            "label": "Car",
            "categoryAttributes": [
                {
                    "name":"X",
                    "description":"enter a number",
                    "type":"number",
                },
                {
                    "name":"Y",
                    "description":"select an option",
                    "type":"string",
                    "enum": ["y1", "y2"]
                },
                {
                    "name":"Z",
                    "description":"submit a free-form response",
                    "type":"string",
                }
            ]
        },
        {
            "label": "Pedestrian",
            "categoryAttributes": [...]
        }
    ],
    "annotationType":"Keypoint",
    "instructions": {"shortInstruction":"add example short instructions here", "fullInstruction":"<html markup>"},
    // include auditLabelAttributeName for label adjustment jobs
    "auditLabelAttributeName": "myPrevJobLabelAttributeName"
}
```

See the quotas for these parameters in [Label and label category attribute quotas](#sms-point-cloud-label-cat-limits)\.

## Creating Worker Instructions<a name="sms-point-cloud-worker-instructions-create"></a>

Create custom instructions for labeling jobs to improve your worker's accuracy in completing their task\. Your instructions are accessible when workers select the **Instructions** menu option in the worker UI\. Short instructions must be under 255 characters and long instruction must be under 2,048 characters\. 

There are two kinds of instructions:
+ **Short instructions** – These instructions are shown to works when they select **Instructions** in the worker UI menu\. They should provide an easy reference to show the worker the correct way to label an object\.
+ **Full instructions** – These instructions are shown when workers select **More Instructions** in instructions the pop\-up window\. We recommend that you provide detailed instructions for completing the task with multiple examples showing edge cases and other difficult situations for labeling objects\.

For 3D point cloud and video frame labeling jobs, you can add worker instructions to your label category configuration file\. You can use a single string to create instructions or you can add HTML mark up to customize the appearance of your instructions and add images\. Make sure that any images you include in your instructions are publicly available, or if your instructions are in Amazon S3, that your workers have read\-access so that they can view them\.