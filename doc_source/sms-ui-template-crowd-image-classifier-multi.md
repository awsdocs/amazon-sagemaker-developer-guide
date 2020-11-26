# crowd\-image\-classifier\-multi\-select<a name="sms-ui-template-crowd-image-classifier-multi"></a>

A widget for classifying an image into one ore more categories\. The image can be a JPG, PNG, or GIF file, and has no size limit\. 

The following is an example of an HTML worker task template built using this crowd element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-image-classifier-multi-select
    name="animals"
    categories="['Cat', 'Dog', 'Horse', 'Pig', 'Bird']"
    src="https://images.unsplash.com/photo-1509205477838-a534e43a849f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1998&q=80"
    header="Please identify the animals in this image"
    exclusion-category="{ text: 'None of the above' }"
  >
    <full-instructions header="Classification Instructions">
      <p>If more than one label applies to the image, select multiple labels.</p>
      <p>If no labels apply, select <b>None of the above</b></p>
    </full-instructions>

    <short-instructions>
      <p>Read the task carefully and inspect the image.</p>
      <p>Choose the appropriate label(s) that best suit the image.</p>
    </short-instructions>
  </crowd-image-classifier-multi-select>
</crowd-form>
```

### Attributes<a name="image-classifier-multi-attributes"></a>

The following attributes are supported by the `crowd-image-classifier-multi-select` element\. Each attribute accepts a string value or string values\.

#### categories<a name="image-classifier-multi-attributes-categories"></a>

Required\. A JSON\-formatted array of strings, each of which is a category that a worker can assign to the image\. A worker must choose at least one category and can choose all categories\. 

#### header<a name="image-classifier-multi-attributes-header"></a>

Required\. The text to display above the image\. This is typically a question or simple instruction for workers\.

#### name<a name="image-classifier-multi-attributes-name"></a>

Required\. The name of this widget\. In the form output, the name is used as a key for the widget's input\.

#### src<a name="image-classifier-multi-attributes-src"></a>

Required\. The URL of the image to be classified\. 

#### exclusion\-category<a name="image-classifier-multi-attributes-exclusion-category"></a>

Optional\. A JSON\-formatted string with the following format: `"{ text: 'default-value' }"`\. This attribute sets a default value that workers can choose if none of the labels applies to the image shown in the worker UI\.

### Element Hierarchy<a name="image-classifier-multi-element-hierarchy"></a>

This element has the following parent and child elements:
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](sms-ui-template-crowd-image-classifier.md#image-classifier-regions-full-instructions), [short\-instructions](sms-ui-template-crowd-image-classifier.md#image-classifier-regions-short-instructions), [worker\-comment](sms-ui-template-crowd-image-classifier.md#image-classifier-regions-worker-comment)

### Regions<a name="image-classifier-multi-regions"></a>

This element uses the following regions

#### full\-instructions<a name="image-classifier-multi-regions-full-instructions"></a>

General instructions for the worker on how to classify an image\.

#### short\-instructions<a name="image-classifier-multi-regions-short-instructions"></a>

Important task\-specific instructions\. These instructions are displayed prominently\.

### Output<a name="image-classifier-multi-output"></a>

The output of this element is a string that specifies one or more of the values defined in the `categories` attribute of the `<crowd-image-classifier-multi-select>` element\.

**Example : Sample Element Outputs**  
The following is a sample of output from this element\.  

```
[
  {
    "<name>": {
        labels: ["label_a", "label_b"]
    }
  }
]
```

### See Also<a name="image-classifier-multi-see-also"></a>

For more information, see the following:
+ [Image Classification \(Multi\-label\)](sms-image-classification-multilabel.md)
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)