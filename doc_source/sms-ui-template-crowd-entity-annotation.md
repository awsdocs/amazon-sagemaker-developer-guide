# crowd\-entity\-annotation<a name="sms-ui-template-crowd-entity-annotation"></a>

A widget for labeling words, phrases, or character strings within a longer text\. Workers select a label, and highlight the text that the label applies to\. 

**Important: Self\-contained Widget**  
Do not use `<crowd-entity-annotation>` element with the `<crowd-form>` element\. It contains its own form submission logic and **Submit** button\.

The following is an example of a template that uses the `<crowd-entity-annotation>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-entity-annotation
  name="crowd-entity-annotation"
  header="Highlight parts of the text below"
  labels="[{'label': 'person', 'shortDisplayName': 'per', 'fullDisplayName': 'Person'}, {'label': 'date', 'shortDisplayName': 'dat', 'fullDisplayName': 'Date'}, {'label': 'company', 'shortDisplayName': 'com', 'fullDisplayName': 'Company'}]"
  text="Amazon SageMaker Ground Truth helps you build highly accurate training datasets for machine learning quickly."
>
  <full-instructions header="Named entity recognition instructions">
    <ol>
      <li><strong>Read</strong> the text carefully.</li>
      <li><strong>Highlight</strong> words, phrases, or sections of the text.</li>
      <li><strong>Choose</strong> the label that best matches what you have highlighted.</li>
      <li>To <strong>change</strong> a label, choose highlighted text and select a new label.</li>
      <li>To <strong>remove</strong> a label from highlighted text, choose the X next to the abbreviated label name on the highlighted text.</li>
      <li>You can select all of a previously highlighted text, but not a portion of it.</li>
    </ol>
  </full-instructions>

  <short-instructions>
    Apply labels to words or phrases.
  </short-instructions>

    <div id="additionalQuestions" style="margin-top: 20px">
      <h3>
        What is the overall subject of this text?
      </h3>
      <crowd-radio-group>
        <crowd-radio-button name="tech" value="tech">Technology</crowd-radio-button>
        <crowd-radio-button name="politics" value="politics">Politics</crowd-radio-button>
      </crowd-radio-group>
    </div>
</crowd-entity-annotation>

<script>
  document.addEventListener('all-crowd-elements-ready', () => {
    document
      .querySelector('crowd-entity-annotation')
      .shadowRoot
      .querySelector('crowd-form')
      .form
      .appendChild(additionalQuestions);
  });
</script>
```

### Attributes<a name="entity-annotation-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="entity-annotation-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### initial\-value<a name="entity-annotation-attributes-initial-value"></a>

A JSON formatted array of objects, each of which defines an annotation to apply to the text at initialization\. Objects contain a `label` value that matches one in the `labels` attribute, an integer `startOffset` value for labeled span's starting unicode offset, and an integer `endOffset` value for the ending unicode offset\.

**Example**  

```
[
  {
    label: 'person',
    startOffset: 0,
    endOffset: 16
  },
  ...
]
```

#### labels<a name="entity-annotation-attributes-labels"></a>

A JSON formatted array of objects, each of which contains:
+ `label` \(required\): The name used to identify entities\.
+ `fullDisplayName` \(optional\): Used for the label list in the task widget\. Defaults to the label value if not specified\.
+ `shortDisplayName` \(optional\): An abbreviation of 3\-4 letters to display above selected entities\. Defaults to the label value if not specified\.
**`shortDisplayName` is highly recommended**  
Values displayed above the selections can overlap and create difficulty managing labeled entities in the workspace\. Providing a 3\-4 character `shortDisplayName` for each label is highly recommended to prevent overlap and keep the workspace manageable for your workers\.

**Example**  

```
[
  {
    label: 'person',
    shortDisplayName: 'per', 
    fullDisplayName: 'person'
  }
]
```

#### name<a name="entity-annotation-attributes-name"></a>

Serves as the widget's name in the DOM\. It is also used as the label attribute name in form output and the output manifest\.

#### text<a name="entity-annotation-attributes-text"></a>

The text to be annotated\. The templating system escapes quotes and HTML strings by default\. If your code is already escaped or partially escaped, see [Variable filters](sms-custom-templates-step2.md#sms-custom-templates-step2-automate-filters) for more ways to control escaping\.

### Element Hierarchy<a name="entity-annotation-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Child elements**: [full\-instructions](#entity-annotation-regions-full-instructions), [short\-instructions](#entity-annotation-regions-short-instructions)

### Regions<a name="entity-annotation-regions"></a>

The following regions are supported by this element\.

#### full\-instructions<a name="entity-annotation-regions-full-instructions"></a>

General instructions about how to work with the widget\.

#### short\-instructions<a name="entity-annotation-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Output<a name="entity-annotation-output"></a>

The following output is supported by this element\.

#### entities<a name="entity-annotation-output-entities"></a>

A JSON object that specifies the start, end, and label of an annotation\. This object contains the following properties\.
+ **label** – The assigned label\.
+ **startOffset** – The Unicode offset of the beginning of the selected text\.
+ **endOffset** – The Unicode offset of the first character after the selection\.

**Example : Sample Element Outputs**  
The following is a sample of the output from this element\.  

```
{
  "myAnnotatedResult": {
    "entities": [
      {
        "endOffset": 54,
        "label": "person",
        "startOffset": 47
      },
      {
        "endOffset": 97,
        "label": "event",
        "startOffset": 93
      },
      {
        "endOffset": 219,
        "label": "date",
        "startOffset": 212
      },
      {
        "endOffset": 271,
        "label": "location",
        "startOffset": 260
      }
    ]
  }
}
```

### See Also<a name="entity-annotation-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)