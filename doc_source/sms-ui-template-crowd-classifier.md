# crowd\-classifier<a name="sms-ui-template-crowd-classifier"></a>

A widget for classifying non\-image content, such as audio, video, or text\.

The following is an example of an HTML worker task template built using `crowd-classifier`\. This example uses the [Liquid template language](https://shopify.github.io/liquid/basics/introduction/) to automate:
+ Label categories in the `categories` parameter 
+ The objects that are being classified in the `classification-target` parameter\. 

Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
    <crowd-classifier
      name="category"
      categories="{{ task.input.labels | to_json | escape }}"
      header="What type of a document is this?"
    >
      <classification-target>
        <iframe style="width: 100%; height: 600px;" src="{{ task.input.taskObject  | grant_read_access }}" type="application/pdf"></iframe>
      </classification-target>
      
      <full-instructions header="Document Classification Instructions">
        <p>Read the task carefully and inspect the document.</p>
        <p>Choose the appropriate label that best suits the document.</p>
      </full-instructions>

      <short-instructions>
        Please choose the correct category for the document
      </short-instructions>
    </crowd-classifier>
</crowd-form>
```

### Attributes<a name="crowd-classifier-attributes"></a>

The following attributes are supported by this element\.

#### categories<a name="crowd-classifier-attributes-categories"></a>

A JSON formatted array of strings, each of which is a category that a worker can assign to the text\. You should include "other" as a category, otherwise the worker my not be able to provide an answer\.

#### header<a name="crowd-classifier-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### name<a name="crowd-classifier-attributes-name"></a>

The name of this widget\. It is used as a key for the widget's input in the form output\.

### Element Hierarchy<a name="crowd-classifier-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [classification\-target](#crowd-classifier-regions-classification-target), [full\-instructions](#crowd-classifier-regions-full-instructions), [short\-instructions](#crowd-classifier-regions-short-instructions)

### Regions<a name="crowd-classifier-regions"></a>

The following regions are supported by this element\.

#### classification\-target<a name="crowd-classifier-regions-classification-target"></a>

The content to be classified by the worker\. This can be plain text or HTML\. Examples of how the HTML can be used include *but are not limited to* embedding a video or audio player, embedding a PDF, or performing a comparison of two or more images\.

#### full\-instructions<a name="crowd-classifier-regions-full-instructions"></a>

General instructions about how to do text classification\.

#### short\-instructions<a name="crowd-classifier-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Output<a name="crowd-classifier-output"></a>

The output of this element is an object using the specified `name` value as a property name, and a string from the `categories` as the property's value\.

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

### See Also<a name="crowd-classifier-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)