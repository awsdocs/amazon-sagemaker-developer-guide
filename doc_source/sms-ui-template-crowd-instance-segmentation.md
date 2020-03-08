# crowd\-instance\-segmentation<a name="sms-ui-template-crowd-instance-segmentation"></a>

A widget for identifying individual instances of specific objects within an image and creating a colored overlay for each labeled instance\.

### Attributes<a name="instance-segmentation-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="instance-segmentation-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### labels<a name="instance-segmentation-attributes-labels"></a>

A JSON formatted array of strings, each of which is a label that a worker can assign to an instance of an object in the image\. Workers can generate different overlay colors for each relevant instance by selecting "add instance" under the label in the tool\.

#### name<a name="instance-segmentation-attributes-name"></a>

The name of this widget\. It is used as a key for the labeling data in the form output\.

#### src<a name="instance-segmentation-attributes-src"></a>

The URL of the image that is to be labeled\.

### Element Hierarchy<a name="instance-segmentation-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#instance-segmentation-regions-full-instructions), [short\-instructions](#instance-segmentation-regions-short-instructions)

### Regions<a name="instance-segmentation-regions"></a>

The following regions are supported by this element\.

#### full\-instructions<a name="instance-segmentation-regions-full-instructions"></a>

General instructions about how to do image segmentation\.

#### short\-instructions<a name="instance-segmentation-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Output<a name="instance-segmentation-output"></a>

The following output is supported by this element\.

#### labeledImage<a name="instance-segmentation-output-labeledImage"></a>

A JSON Object containing a Base64 encoded PNG of the labels\.

#### instances<a name="instance-segmentation-output-labelMappings"></a>

A JSON Array containing objects with the instance labels and colors\.
+ **color** – The hexadecimal value of the label's RGB color in the `labeledImage` PNG\.
+ **label** – The label given to overlay\(s\) using that color\. This value may repeat, because the different instances of the label are identified by their unique color\.

#### inputImageProperties<a name="instance-segmentation-output-inputImageProperties"></a>

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
      "instances": [
        {
          "color": "#1f77b4",
          "label": "<Label 1>": 
        },
        {
          "color": "#2ca02c",
          "label": "<Label 1>": 
        },
        {
          "color": "#ff7f0e",
          "label": "<Label 3>": 
        },
      ],
      "labeledImage": {
        "pngImageData": "<Base-64 Encoded Data>"
      }
    }
  }
]
```

### See Also<a name="instance-segmentation-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth for Labeling](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)