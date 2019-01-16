# crowd\-bounding\-box<a name="sms-ui-template-crowd-bounding-box"></a>

A widget for drawing rectangles on an image and assigning a label to the portion of the image that is enclosed in each rectangle\.

## Attributes<a name="bounding-box-attributes"></a>

The following attributes are supported by this element\.

### header<a name="bounding-box-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

### labels<a name="bounding-box-attributes-labels"></a>

A JSON formatted array of strings, each of which is a label that a worker can assign to the image portion enclosed by a rectangle\.

### name<a name="bounding-box-attributes-name"></a>

The name of this widget\. It's used as a key for the widget's input in the form output\.

### src<a name="bounding-box-attributes-src"></a>

The URL of the image on which to draw bounding boxes\. 

### initialValue<a name="bounding-box-attributes-initialValue"></a>

An array of JSON objects, each of which sets a bounding box when the component is loaded\. Each JSON object in the array contains the following properties\.
+ **height** – The height of the box in pixels\.
+ **label** – The text assigned to the box as part of the labeling task\. This text must match one of the labels defined in the *labels* attribute of the <crowd\-bounding\-box> element\.
+ **left** – Distance of the top\-left corner of the box from the left side of the image, measured in pixels\.
+ **top** – Distance of the top\-left corner of the box from the top of the image, measured in pixels\.
+ **width** – The width of the box in pixels\.

## Element Hierarchy<a name="bounding-box-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#bounding-box-regions-full-instructions), [short\-instructions](#bounding-box-regions-short-instructions)

## Regions<a name="bounding-box-regions"></a>

The following regions are required by this element\.

### full\-instructions<a name="bounding-box-regions-full-instructions"></a>

General instructions about how to draw bounding boxes\.

### short\-instructions<a name="bounding-box-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

## Output<a name="bounding-box-output"></a>

The following output is supported by this element\.

### boundingBoxes<a name="bounding-box-output-boundingBoxes"></a>

An array of JSON objects, each of which specifies a bounding box that has been created by the worker\. Each JSON object in the array contains the following properties\.
+ **height** – The height of the box in pixels\.
+ **label** – The text assigned to the box as part of the labeling task\. This text must match one of the labels defined in the *labels* attribute of the <crowd\-bounding\-box> element\.
+ **left** – Distance of the top\-left corner of the box from the left side of the image, measured in pixels\.
+ **top** – Distance of the top\-left corner of the box from the top of the image, measured in pixels\.
+ **width** – The width of the box in pixels\.

### inputImageProperties<a name="bounding-box-output-inputImageProperties"></a>

A JSON object that specifies the dimensions of the image that is being annotated by the worker\. This object contains the following properties\.
+ **height** – The height, in pixels, of the image\.
+ **width** – The width, in pixels, of the image\.

**Example : Sample Element Outputs**  
The following are samples of outputs from common use scenarios for this element\.  
**Single Label, Single Box / Multiple Label, Single Box**  

```
[
  {
    "annotatedResult": {
      "boundingBoxes": [
        {
          "height": 401,
          "label": "Dog",
          "left": 243,
          "top": 117,
          "width": 187
        }
      ],
      "inputImageProperties": {
        "height": 533,
        "width": 800
      }
    }
  }
]
```
**Single Label, Multiple Box**  

```
[
  {
    "annotatedResult": {
      "boundingBoxes": [
        {
          "height": 401,
          "label": "Dog",
          "left": 243,
          "top": 117,
          "width": 187
        },
        {
          "height": 283,
          "label": "Dog",
          "left": 684,
          "top": 120,
          "width": 116
        }
      ],
      "inputImageProperties": {
        "height": 533,
        "width": 800
      }
    }
  }
]
```
**Multiple Label, Multiple Box**  

```
[
  {
    "annotatedResult": {
      "boundingBoxes": [
        {
          "height": 395,
          "label": "Dog",
          "left": 241,
          "top": 125,
          "width": 158
        },
        {
          "height": 298,
          "label": "Cat",
          "left": 699,
          "top": 116,
          "width": 101
        }
      ],
      "inputImageProperties": {
        "height": 533,
        "width": 800
      }
    }
  }
]
```
You could have many labels available, but only the ones that are used appear in the output\.

## See Also<a name="bounding-box-see-also"></a>

For more information, see the following\.
+ [Amazon SageMaker Ground Truth](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)