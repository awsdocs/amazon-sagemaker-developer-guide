# crowd\-semantic\-segmentation<a name="sms-ui-template-crowd-semantic-segmentation"></a>

A widget for segmenting an image and assigning a label to each image segment\.

## Attributes<a name="semantic-segmentation-attributes"></a>

The following attributes are supported by this element\.

### header<a name="semantic-segmentation-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

### labels<a name="semantic-segmentation-attributes-labels"></a>

A JSON formatted array of strings, each of which is a label that a worker can assign to a segment of the image\.

### name<a name="semantic-segmentation-attributes-name"></a>

The name of this widget\. It is used as a key for the widget's input in the form output\.

### src<a name="semantic-segmentation-attributes-src"></a>

The URL of the image that is to be segmented\.

## Element Hierarchy<a name="semantic-segmentation-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#semantic-segmentation-regions-full-instructions), [short\-instructions](#semantic-segmentation-regions-short-instructions)

## Regions<a name="semantic-segmentation-regions"></a>

The following regions are supported by this element\.

### full\-instructions<a name="semantic-segmentation-regions-full-instructions"></a>

General instructions about how to do image segmentation\.

### short\-instructions<a name="semantic-segmentation-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

## Output<a name="semantic-segmentation-output"></a>

The following output is supported by this element\.

### labeledImage<a name="semantic-segmentation-output-labeledImage"></a>

A JSON Object containing a Base64 encoded PNG of the labels\.

### labelMappings<a name="semantic-segmentation-output-labelMappings"></a>

A JSON Object containing objects with named with the segmentation labels\.
+ **color** – The hexadecimal value of the label's RGB color in the `labeledImage` PNG\.

### inputImageProperties<a name="semantic-segmentation-output-inputImageProperties"></a>

A JSON object that specifies the dimensions of the image that is being annotated by the worker\. This object contains the following properties\.
+ **height** – The height, in pixels, of the image\.
+ **width** – The width, in pixels, of the image\.

**Example : Sample Element Outputs**  
The following is a sample of output from this element\.  

```
[
  {
    "annotatedResult": {
      "inputImageProperties": {
        "height": 533,
        "width": 800
      },
      "labelMappings": {
        "<Label 2>": {
          "color": "#ff7f0e"
        },
        "<label 3>": {
          "color": "#2ca02c"
        },
        "<label 1>": {
          "color": "#1f77b4"
        }
      },
      "labeledImage": {
        "pngImageData": "<Base-64 Encoded Data>"
      }
    }
  }
]
```

## See Also<a name="semantic-segmentation-see-also"></a>

For more information, see the following\.
+ [Amazon SageMaker Ground Truth](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)