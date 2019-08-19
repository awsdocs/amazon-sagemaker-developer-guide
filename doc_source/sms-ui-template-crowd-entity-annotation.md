# crowd\-entity\-annotation<a name="sms-ui-template-crowd-entity-annotation"></a>

A widget for labeling words, phrases, or character strings within a longer text\.

**Important: Self\-contained Widget**  
Do not use `<crowd-entity-annotation>` element with the `<crowd-form>` element\. It contains its own form submission logic and **Submit** button\.

## Attributes<a name="entity-annotation-attributes"></a>

The following attributes are supported by this element\.

### header<a name="entity-annotation-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

### initial\-value<a name="entity-annotation-attributes-initial-value"></a>

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

### labels<a name="entity-annotation-attributes-labels"></a>

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

### name<a name="entity-annotation-attributes-name"></a>

Serves as the widget's name in the DOM\. It is also used as the label attribute name in form output and the output manifest\.

### text<a name="entity-annotation-attributes-text"></a>

The text to be annotated\. The templating system escapes quotes and HTML strings by default\. If your code is already escaped or partially escaped, see [Variable filters](sms-custom-templates-step2.md#sms-custom-templates-step2-automate-filters) for more ways to control escaping\.

## Element Hierarchy<a name="entity-annotation-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Child elements**: [full\-instructions](#entity-annotation-regions-full-instructions), [short\-instructions](#entity-annotation-regions-short-instructions)

## Regions<a name="entity-annotation-regions"></a>

The following regions are supported by this element\.

### full\-instructions<a name="entity-annotation-regions-full-instructions"></a>

General instructions about how to work with the widget\.

### short\-instructions<a name="entity-annotation-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

## Output<a name="entity-annotation-output"></a>

The following output is supported by this element\.

### entities<a name="entity-annotation-output-entities"></a>

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

## See Also<a name="entity-annotation-see-also"></a>

For more information, see the following\.
+ [Amazon SageMaker Ground Truth](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)