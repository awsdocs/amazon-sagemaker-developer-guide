# Create a Labeling Category Configuration File with Label Category Attributes<a name="sms-label-cat-config-attributes"></a>

When you create a 3D point cloud or video frame labeling job using the Amazon SageMaker API operation `CreateLabelingJob`, you use a label category configuration file to specify your labels and worker instructions\. Optionally, you can provide label category attributes for video frame and 3D point cloud object tracking and object detection task types\. Workers can assign one or more attributes to annotations to give more information about that object\. For example, you may want to use the attribute *occluded* to have workers identify when an object is partially obstructed\. You can either specify a label category attribute for a single label using the `categoryAttributes` parameter, or for all labels using the `categoryGlobalAttributes` parameter\. 

Workers can select multiple attributes for each label, up to the total number of attributes you provide for that label\. 

The following is an example of a label category configuration file that includes label category attributes\. See the quotas for these parameters in [Label and label category attribute quotas](#sms-point-cloud-label-cat-limits)\.

```
{
    "documentVersion": "2020-03-01",
    "categoryGlobalAttributes": [
        {
            "name":"applyToAllCategory",
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
                    "description":"something",
                    "type":"string",
                    "enum": ["foo", "buz", "buzz2"]
                },
                {
                    "name":"Y",
                    "description":"something",
                    "type":"string",
                    "enum": ["y1", "y2"]
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

Label category attributes are not supported for 3D point cloud semantic segmentation task types\. If you provide label category attributes for a semantic segmentation labeling job, they will be ignored\. Your label category configure file for a 3D point cloud semantic segmentation labeling job may look similar to the following:

```
{
    "documentVersion": "2020-03-01",
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

**Important**  
You should only provide a label attribute name in `auditLabelAttributeName` if you are running an audit job to verify or adjust labels\. Use this parameter to input the [LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) used in the labeling job that generated the annotations you want your worker to adjust\. When you create a labeling job in the console, if you did not specify a label attribute name, the **Name** of your job is used as the LabelAttributeName\.

The following table lists elements you can and must include in your label category configuration file\.


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
| categoryGlobalAttributes |  No  |  A list of JSON objects Each object can contain the following elements: `name`, `type`, `enum`\. **Required Parameters in each JSON Object:** `name`, `type` **Optional Parameters in each JSON Object:** `description`, `enum`  |  Use this parameter to create a label category attribute that is applied to all labels you specify in `labels`\.  The attribute is called `name` in the worker UI, and the worker can assign one value from `enum` to this attribute\.  The `type` parameter must be `string`\. Use the `description` parameter to provide more information about your label category attribute\.   | 
| labels |  Yes  |  A list of up to 30 JSON objects **Required Parameters in each JSON Object:** `label` **Optional Parameters in each JSON Object:** `categoryAttributes`  |  Use this parameter to specify your labels, or classes\. Add one `label` for each class\.  To add a label category attribute to a label, add `categoryAttributes` to that label\.  | 
| categoryAttributes |  No  |  A list of up to 10 JSON objects **Required Parameters in each JSON Object:** `name`, `type` **Optional Parameters in each JSON Object:** `description`, `enum`  |  Use this parameter to add label category attributes to a specific labels you specify in `label` in the same JSON object under `labels`\.  Each attribute you add to this list is called `name` in the worker UI, and the worker can assign one value from `enum` to the attribute\. The `type` parameter must be `string`\. Use the `description` parameter to provide more information about your label category attribute\.   | 
| instructions |  No  | A JSON objectRequired Parameters in each JSON Object:`shortInstruction`, `fullInstruction` |  Use this parameter to add worker instructions to help your workers complete their tasks\. For more information about worker instructions, see [Worker Instructions](sms-point-cloud-general-information.md#sms-point-cloud-worker-instructions-general)\.  Short instructions must be under 255 characters and long instruction must be under 2,048 characters\.  For more information, see [Creating Worker Instructions](#sms-point-cloud-worker-instructions-create)\.  | 
| auditLabelAttributeName |  For Adjustment Task Types  |  String  |  Enter the [LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) used in the labeling job you want to adjust annotations of\.  Only use this parameter if you are creating an adjustment job for video frame and 3D point cloud object detection, object tracking, or 3D point cloud semantic segmentation\.   | 

The following table describes the parameters that you can and must use to describe a specific label category attribute in the `categoryGlobalAttributes` and `categoryAttributes` parameters\. Note that both of these parameters are optional in the previous table\. 


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
| name |  Yes  |  String  |  Use this parameter to assign a name to your label category attribute\. This is the name that workers see for this attribute\.   | 
| type |  Yes  |  String **Required Values**: `string`  |  Use this parameter to define the label category attribute type\.   | 
| description |  No  |  String  |   Use this parameter to add a description of the label category attribute\.   | 
| enum |  No  |  List of strings  |  Use this parameter to define options that workers can choose from for this label category attribute\. Workers can choose one value specified in `enum`\.   | 

## Label and label category attribute quotas<a name="sms-point-cloud-label-cat-limits"></a>

You can specify up to 10 label category attributes per class\. This 10\-attribute quotas includes global label category attributes\. For example, if you create four global label category attributes, and then assign three label category attributes to label `X`, that label will have 4\+3=7 label category attributes in total\. For all label category and label category attribute limits, refer to the following table\. 


****  

|  Type  |  Min  |  Max  | 
| --- | --- | --- | 
|  Labels \(`Labels`\)  |  0  |  30  | 
|  Label name character limit  |  1  |  16  | 
|  Attributes per label \(including `categoryAttributes` and `categoryGlobalAttributes`\)  |  0  |  10  | 
|  Attribute name character limit \(`name`\)  |  1  |  16  | 
|  Attribute description character limit \(`description`\)  |  0  |  128  | 
|  Attribute type characters limit \(`type`\)  |  1  |  16  | 

## Creating Worker Instructions<a name="sms-point-cloud-worker-instructions-create"></a>

Create custom instructions for labeling jobs to improve your worker's accuracy in completing their task\. Your instructions are accessible when workers select the **Instructions** menu option in the worker UI\. Short instructions must be under 255 characters and long instruction must be under 2,048 characters\. 

There are two kinds of instructions:
+ **Short instructions** – These instructions are shown to works when they select **Instructions** in the worker UI menu\. They should provide an easy reference to show the worker the correct way to label an object\.
+ **Full instructions** – These instructions are shown when workers select **More Instructions** in instructions the pop\-up window\. We recommend that you provide detailed instructions for completing the task with multiple examples showing edge cases and other difficult situations for labeling objects\.

For 3D point cloud and video frame labeling jobs, you can add worker instructions to your label category configuration file\. You can use a single string to create instructions or you can add HTML mark up to customize the appearance of your instructions and add images\. Make sure that any images you include in your instructions are publicly available, or if your instructions are in Amazon S3, that your workers have read\-access so that they can view them\.