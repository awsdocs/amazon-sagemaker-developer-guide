# crowd\-image\-classifier<a name="sms-ui-template-crowd-image-classifier"></a>

A widget for classifying an image, which can be a JPG, PNG, or GIF, with no size limit\.

The following is an example of an image classification template that uses the `<crowd-image-classifier>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
    <crowd-image-classifier 
        src="${image_url}"
        categories="['Cat', 'Dog', 'Bird', 'None of the Above']"
        header="Choose the correct category for the image"
        name="category">


        <short-instructions>
            <p>Read the task carefully and inspect the image.</p>
            <p>Choose the appropriate label that best suits the image.</p>
        </short-instructions>

 
        <full-instructions header="Classification Instructions">
            <p>Read the task carefully and inspect the image.</p>
            <p>Choose the appropriate label that best suits the image. 
            Use the <b>None of the Above</b> option if none of the other labels suit the image.</p>
        </full-instructions>

    </crowd-image-classifier>
</crowd-form>
```

### Attributes<a name="image-classifier-attributes"></a>

The following attributes are required by this element\.

#### categories<a name="image-classifier-attributes-categories"></a>

A JSON formatted array of strings, each of which is a category that a worker can assign to the image\. You should include "other" as a category, so that the worker can provide an answer\. You can specify up to 10 categories\.

#### header<a name="image-classifier-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### name<a name="image-classifier-attributes-name"></a>

The name of this widget\. It is used as a key for the widget's input in the form output\.

#### overlay<a name="image-classifier-attributes-overlay"></a>

Information to be overlaid on the source image\. This is for verification workflows of bounding\-box, semantic\-segmentation, and instance\-segmentation tasks\.

It is a JSON object containing an object with the name of the task\-type in camelCase as the key\. That key's value is an object that contains the labels and other necessary information from the previous task\.

An example of a `crowd-image-classifier` element with attributes for verifying a bounding\-box task follows:

```
<crowd-image-classifier
    name="boundingBoxClassification"
    header="Rate the quality of the annotations based on the background section 
       in the instructions on the left hand side."
    src="https://i.imgur.com/CIPKVJo.jpg"
    categories="['good', 'bad', 'okay']"
    overlay='{
        "boundingBox": {
            labels: ["bird", "cat"], 
            value: [
                {
                  height: 284,
                  label: "bird",
                  left: 230,
                  top: 974,
                  width: 223
                },
                {
                  height: 69,
                  label: "bird",
                  left: 79,
                  top: 889,
                  width: 247
                }
            ]
        },
    }'
> ... </crowd-image-classifier>
```

A semantic segmentation verification task would use the `overlay` value as follows:

```
<crowd-image-classifier
  name='crowd-image-classifier'
  categories='["good", "bad"]'
  src='URL of image to be classified'
  header='Please classify'
  overlay='{
    "semanticSegmentation": {
      "labels": ["Cat", "Dog", "Bird", "Cow"],
      "labelMappings": {
        "Bird": {
          "color": "#ff7f0e"
        },
        "Cat": {
          "color": "#2ca02c"
        },
        "Cow": {
          "color": "#d62728"
        },
        "Dog": {
          "color": "#2acf59"
        }
      },
      "src": "URL of overlay image",
    }
  }'
> ... </crowd-image-classifier>
```

An instance\-segmentation task would use the `overlay` value as follows:

```
<crowd-image-classifier
  name='crowd-image-classifier'
  categories='["good", "bad"]'
  src='URL of image to be classified'
  header='Please classify instances of each category'
  overlay='{
    "instanceSegmentation": {
       "labels": ["Cat", "Dog", "Bird", "Cow"],
       "instances": [
        {
         "color": "#2ca02c",
         "label": "Cat"
        },
        {
         "color": "#1f77b4",
         "label": "Cat"
        },
        {
         "color": "#d62728",
         "label": "Dog"
        }
       ],
       "src": "URL of overlay image",
    }
  }'
> ... </crowd-image-classifier>
```

#### src<a name="image-classifier-attributes-src"></a>

The URL of the image to be classified\. 

### Element Hierarchy<a name="image-classifier-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#image-classifier-regions-full-instructions), [short\-instructions](#image-classifier-regions-short-instructions), [worker\-comment](#image-classifier-regions-worker-comment)

### Regions<a name="image-classifier-regions"></a>

The following regions are used by this element\.

#### full\-instructions<a name="image-classifier-regions-full-instructions"></a>

General instructions for the worker on how to classify an image\.

#### short\-instructions<a name="image-classifier-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

#### worker\-comment<a name="image-classifier-regions-worker-comment"></a>

Use this in verification workflows when you need workers to explain why they made the choice they did\. Use the text between the opening and closing tags to provide instructions for workers on what information should be included in the comment\.

It uses the following attributes:

##### header<a name="image-classifier-regions-worker-comment-header"></a>

A phrase with a call to action for leaving a comment\. Used as the title text for a modal window where the comment is added\.

Optional\. Defaults to "Add a comment\."

##### link\-text<a name="image-classifier-regions-worker-comment-link-text"></a>

This text appears below the categories in the widget\. When clicked, it opens a modal window where the worker may add a comment\.

Optional\. Defaults to "Add a comment\."

##### placeholder<a name="image-classifier-regions-worker-comment-placeholder"></a>

An example text in the comment text area that is overwritten when worker begins to type\. This does not appear in output if the worker leaves the field blank\.

Optional\. Defaults to blank\.

### Output<a name="image-classifier-output"></a>

The output of this element is a string that specifies one of the values defined in the *categories* attribute of the <crowd\-image\-classifier> element\.

**Example : Sample Element Outputs**  
The following is a sample of output from this element\.  

```
[
  {
    "<name>": {
      "label": "<value>"
      "workerComment": "Comment - if no comment is provided, this field will not be present"
    }
  }
]
```

### See Also<a name="image-classifier-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)