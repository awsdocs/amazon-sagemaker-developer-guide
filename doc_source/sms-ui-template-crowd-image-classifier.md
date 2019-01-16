# crowd\-image\-classifier<a name="sms-ui-template-crowd-image-classifier"></a>

A widget for classifying an image, which can be a JPG, PNG, or GIF, with no size limit\.

## Attributes<a name="image-classifier-attributes"></a>

The following attributes are required by this element\.

### categories<a name="image-classifier-attributes-categories"></a>

A JSON formatted array of strings, each of which is a category that a worker can assign to the image\. You should include "other" as a category, so that the worker can provide an answer\. You can specify up to 10 categories\.

### header<a name="image-classifier-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

### name<a name="image-classifier-attributes-name"></a>

The name of this widget\. It is used as a key for the widget's input in the form output\.

### src<a name="image-classifier-attributes-src"></a>

The URL of the image to be classified\. 

## Element Hierarchy<a name="image-classifier-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#image-classifier-regions-full-instructions), [short\-instructions](#image-classifier-regions-short-instructions)

## Regions<a name="image-classifier-regions"></a>

The following regions are required by this element\.

### full\-instructions<a name="image-classifier-regions-full-instructions"></a>

General instructions about how to do image classification\.

### short\-instructions<a name="image-classifier-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

## Output<a name="image-classifier-output"></a>

The output of this element is a string that specifies one of the values defined in the *categories* attribute of the <crowd\-image\-classifier> element\.

**Example : Sample Element Outputs**  
The following is a sample of output from this element\.  

```
[
  {
    "<name>": {
      "label": "<value>"
    }
  }
]
```

## See Also<a name="image-classifier-see-also"></a>

For more information, see the following\.
+ [Amazon SageMaker Ground Truth](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)