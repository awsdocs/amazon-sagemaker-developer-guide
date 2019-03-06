# crowd\-keypoint<a name="sms-ui-template-crowd-keypoint"></a>

Generates a tool to select and annotate key points on an image\.

## Attributes<a name="keypoint-attributes"></a>

The following attributes are supported by this element\.

### header<a name="keypoint-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

### labels<a name="keypoint-attributes-labels"></a>

An array, in JSON format, of strings to be used as keypoint annotation labels\.

### name<a name="keypoint-attributes-name"></a>

A string used to identify the answer submitted by the worker\. This value will match a key in the JSON object that specifies the answer\.

### src<a name="keypoint-attributes-src"></a>

The source URI of the image to be annotated\.

## Element Hierarchy<a name="keypoint-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#keypoint-regions-full-instructions), [short\-instructions](#keypoint-regions-short-instructions)

## Regions<a name="keypoint-regions"></a>

The following regions are required by this element\.

### full\-instructions<a name="keypoint-regions-full-instructions"></a>

General instructions about how to annotate the image\.

### short\-instructions<a name="keypoint-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

## Output<a name="keypoint-output"></a>

The following output is supported by this element\.

### inputImageProperties<a name="keypoint-output-inputImageProperties"></a>

A JSON object that specifies the dimensions of the image that is being annotated by the worker\. This object contains the following properties\.
+ **height** – The height, in pixels, of the image\.
+ **width** – The width, in pixels, of the image\.

### keypoints<a name="keypoint-output-keypoints"></a>

An array of JSON objects containing the coordinates and label of a keypoint\. Each object contains the following properties\.
+ **label** – The assigned label for the keypoint\.
+ **x** – The X coordinate, in pixels, of the keypoint on the image\.
+ **y** – The Y coordinate, in pixels, of the keypoint on the image\.

**Note**  
X and Y coordinates are based on 0,0 being the top left corner of the image\.

**Example : Sample Element Outputs**  
The following is a sample output from using this element\.  

```
[
  {
    "crowdKeypoint": {
      "inputImageProperties": {
        "height": 1314,
        "width": 962
      },
      "keypoints": [
        {
          "label": "dog",
          "x": 155,
          "y": 275
        },
        {
          "label": "cat",
          "x": 341,
          "y": 447
        },
        {
          "label": "cat",
          "x": 491,
          "y": 513
        },
        {
          "label": "dog",
          "x": 714,
          "y": 578
        },
        {
          "label": "cat",
          "x": 712,
          "y": 763
        },
        {
          "label": "cat",
          "x": 397,
          "y": 814
        }
      ]
    }
  }
]
```
You may have many labels available, but only the ones that are used appear in the output\.

## See Also<a name="keypoint-see-also"></a>

For more information, see the following\.
+ [Amazon SageMaker Ground Truth](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)