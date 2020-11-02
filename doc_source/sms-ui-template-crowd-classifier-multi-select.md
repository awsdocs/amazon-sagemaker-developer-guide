# crowd\-classifier\-multi\-select<a name="sms-ui-template-crowd-classifier-multi-select"></a>

A widget for classifying various forms of content—such as audio, video, or text—into one or more categories\. The content to classify is referred to as an *object*\. 

The following is an example of an HTML worker task template built using this crowd element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
    <crowd-classifier-multi-select
      name="category"
      categories="['Positive', 'Negative', 'Neutral']"
      header="Select the relevant categories"
      exclusion-category="{ text: 'None of the above' }"
    >
      <classification-target>
        {{ task.input.taskObject }}
      </classification-target>
      
      <full-instructions header="Text Categorization Instructions">
        <p><strong>Positive</strong> sentiment include: joy, excitement, delight</p>
        <p><strong>Negative</strong> sentiment include: anger, sarcasm, anxiety</p>
        <p><strong>Neutral</strong>: neither positive or negative, such as stating a fact</p>
        <p><strong>N/A</strong>: when the text cannot be understood</p>
        <p>When the sentiment is mixed, such as both joy and sadness, choose both labels.</p>
      </full-instructions>

      <short-instructions>
       Choose all categories that are expressed by the text. 
      </short-instructions>
    </crowd-classifier-multi-select>
</crowd-form>
```

### Attributes<a name="crowd-classifier-multi-attributes"></a>

The following attributes are supported by the `crowd-classifier-multi-select` element\. Each attribute accepts a string value or string values\. 

#### categories<a name="crowd-classifier-multi-attributes-categories"></a>

Required\. A JSON\-formatted array of strings, each of which is a category that a worker can assign to the object\. 

#### header<a name="crowd-classifier-multi-attributes-header"></a>

Required\. The text to display above the image\. This is typically a question or simple instruction for workers\.

#### name<a name="crowd-classifier-multi-attributes-name"></a>

Required\. The name of this widget\. In the form output, the name is used as a key for the widget's input\.

#### exclusion\-category<a name="crowd-classifier-multi-attributes-exclusion-category"></a>

Optional\. A JSON\-formatted string with the following format: `"{ text: 'default-value' }"`\. This attribute sets a default value that workers can choose if none of the labels applies to the object shown in the worker UI\.

### Element Hierarchy<a name="crowd-classifier-multi-element-hierarchy"></a>

This element has the following parent and child elements:
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [classification\-target](sms-ui-template-crowd-classifier.md#crowd-classifier-regions-classification-target), [full\-instructions](sms-ui-template-crowd-classifier.md#crowd-classifier-regions-full-instructions), [short\-instructions](sms-ui-template-crowd-classifier.md#crowd-classifier-regions-short-instructions)

### Regions<a name="crowd-classifier-multi-regions"></a>

This element uses the following regions\.

#### classification\-target<a name="crowd-classifier-multi-regions-classification-target"></a>

The content to be classified by the worker\. Content can be plain text or an object that you specify in the template using HTML\. For example, you can use HTML elements to include a video or audio player, embedding a PDF file, or include a comparison of two or more images\.

#### full\-instructions<a name="crowd-classifier-multi-regions-full-instructions"></a>

General instructions about how to classify text\.

#### short\-instructions<a name="crowd-classifier-multi-regions-short-instructions"></a>

Important task\-specific instructions\. These instructions are displayed prominently\.

### Output<a name="crowd-classifier-multi-output"></a>

The output of this element is an object that uses the specified `name` value as a property name, and a string from `categories` as the property's value\.

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

### See Also<a name="crowd-classifier-multi-see-also"></a>

For more information, see the following:
+ [Text Classification \(Multi\-label\)](sms-text-classification-multilabel.md)
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)