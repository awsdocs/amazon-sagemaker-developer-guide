# crowd\-semantic\-segmentation<a name="sms-ui-template-crowd-semantic-segmentation"></a>

A widget for segmenting an image and assigning a label to each image segment\.

The following is an example of a Liquid template that uses the `<crowd-semantic-segmentation>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-semantic-segmentation
    name="annotatedResult"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="Please label each of the requested objects in this image"
    labels="['Cat', 'Dog', 'Bird']"
  >
    <full-instructions header="Segmentation Instructions">
      <ol>
          <li><strong>Read</strong> the task carefully and inspect the image.</li>
          <li><strong>Read</strong> the options and review the examples provided to understand more about the labels.</li>
          <li><strong>Choose</strong> the appropriate label that best suits the image.</li>
      </ol>
    </full-instructions>

    <short-instructions>
      <p>Use the tools to label the requested items in the image</p>
    </short-instructions>
  </crowd-semantic-segmentation>
</crowd-form>
```

### Attributes<a name="semantic-segmentation-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="semantic-segmentation-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### initial\-value<a name="semantic-segmentation-attributes-initial-value"></a>

A JSON object containing the color mappings of a prior semantic segmentation job and a link to the overlay image output by the prior job\. Include this when you want a human worker to verify the results of a prior labeling job and adjust it if necessary\.

The attribute would appear as follows:

```
  initial-value='{
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
          "color": "#1f77b4"
        }
      },
    "src": {{ "S3 file URL for image" | grant_read_access }}
  }'
```

When using Ground Truth [built in task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) with [annotation consolidation](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-annotation-consolidation.html) \(where more than one worker labels a single image\), label mappings are included in individual worker output records, however the overall result is represented as the `internal-color-map` in the consolidated results\.

You can convert the `internal-color-map` to `label-mappings` in a custom template using the Liquid templating language:

```
initial-value="{
  'src' : '{{ task.input.manifestLine.label-attribute-name-from-prior-job| grant_read_access }}',
  'labelMappings': {
     {% for box in task.input.manifestLine.label-attribute-name-from-prior-job-metadata.internal-color-map %}
       {% if box[1]['class-name'] != 'BACKGROUND' %}
         {{ box[1]['class-name'] | to_json }}: {
           'color': {{ box[1]['hex-color'] | to_json }}
         },
       {% endif %} 
     {% endfor %}
   } 
}"
```

#### labels<a name="semantic-segmentation-attributes-labels"></a>

A JSON formatted array of strings, each of which is a label that a worker can assign to a segment of the image\.

#### name<a name="semantic-segmentation-attributes-name"></a>

The name of this widget\. It is used as a key for the widget's input in the form output\.

#### src<a name="semantic-segmentation-attributes-src"></a>

The URL of the image that is to be segmented\.

### Element Hierarchy<a name="semantic-segmentation-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#semantic-segmentation-regions-full-instructions), [short\-instructions](#semantic-segmentation-regions-short-instructions)

### Regions<a name="semantic-segmentation-regions"></a>

The following regions are supported by this element\.

#### full\-instructions<a name="semantic-segmentation-regions-full-instructions"></a>

General instructions about how to do image segmentation\.

#### short\-instructions<a name="semantic-segmentation-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Output<a name="semantic-segmentation-output"></a>

The following output is supported by this element\.

#### labeledImage<a name="semantic-segmentation-output-labeledImage"></a>

A JSON Object containing a Base64 encoded PNG of the labels\.

#### labelMappings<a name="semantic-segmentation-output-labelMappings"></a>

A JSON Object containing objects with named with the segmentation labels\.
+ **color** – The hexadecimal value of the label's RGB color in the `labeledImage` PNG\.

#### initialValueModified<a name="semantic-segmentation-output-initialValueModified"></a>

A boolean representing whether the initial values have been modified\. This is only included when the output is from an adjustment task\.

#### inputImageProperties<a name="semantic-segmentation-output-inputImageProperties"></a>

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

### See Also<a name="semantic-segmentation-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)