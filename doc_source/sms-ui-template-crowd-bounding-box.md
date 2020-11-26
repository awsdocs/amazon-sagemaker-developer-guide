# crowd\-bounding\-box<a name="sms-ui-template-crowd-bounding-box"></a>

A widget for drawing rectangles on an image and assigning a label to the portion of the image that is enclosed in each rectangle\.

The following is an example of a Liquid template that uses the `<crowd-bounding-box>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. For more examples, see this [GitHub repository](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/tree/master/images)\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-bounding-box
    name="annotatedResult"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="Draw bounding boxes around all the cats and dogs in this image"
    labels="['Cat', 'Dog']"
  >
    <full-instructions header="Bounding Box Instructions" >
      <p>Use the bounding box tool to draw boxes around the requested target of interest:</p>
      <ol>
        <li>Draw a rectangle using your mouse over each instance of the target.</li>
        <li>Make sure the box does not cut into the target, leave a 2 - 3 pixel margin</li>
        <li>
          When targets are overlapping, draw a box around each object,
          include all contiguous parts of the target in the box.
          Do not include parts that are completely overlapped by another object.
        </li>
        <li>
          Do not include parts of the target that cannot be seen,
          even though you think you can interpolate the whole shape of the target.
        </li>
        <li>Avoid shadows, they're not considered as a part of the target.</li>
        <li>If the target goes off the screen, label up to the edge of the image.</li>
      </ol>
    </full-instructions>

    <short-instructions>
      Draw boxes around the requested target of interest.
    </short-instructions>
  </crowd-bounding-box>
</crowd-form>
```

### Attributes<a name="bounding-box-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="bounding-box-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### initial\-value<a name="bounding-box-attributes-initialValue"></a>

An array of JSON objects, each of which sets a bounding box when the component is loaded\. Each JSON object in the array contains the following properties\. Bounding boxes set via the `initial-value` property can be adjusted and whether or not a worker answer was adjusted is tracked via an `initialValueModified` boolean in the worker answer output\.
+ **height** – The height of the box in pixels\.
+ **label** – The text assigned to the box as part of the labeling task\. This text must match one of the labels defined in the *labels* attribute of the <crowd\-bounding\-box> element\.
+ **left** – Distance of the top\-left corner of the box from the left side of the image, measured in pixels\.
+ **top** – Distance of the top\-left corner of the box from the top of the image, measured in pixels\.
+ **width** – The width of the box in pixels\.

  You can extract the bounding box initial value from a manifest file of a previous job in a custom template using the Liquid templating language:

  ```
  initial-value="[
    {% for box in task.input.manifestLine.label-attribute-name-from-prior-job.annotations %}
      {% capture class_id %}{{ box.class_id }}{% endcapture %}
      {% assign label = task.input.manifestLine.label-attribute-name-from-prior-job-metadata.class-map[class_id] %}
      {
        label: {{label | to_json}},
        left: {{box.left}},
        top: {{box.top}},
        width: {{box.width}},
        height: {{box.height}},
      },
    {% endfor %}
   ]"
  ```

#### labels<a name="bounding-box-attributes-labels"></a>

A JSON formatted array of strings, each of which is a label that a worker can assign to the image portion enclosed by a rectangle\. **Limit:** 10 labels\.

#### name<a name="bounding-box-attributes-name"></a>

The name of this widget\. It's used as a key for the widget's input in the form output\.

#### src<a name="bounding-box-attributes-src"></a>

The URL of the image on which to draw bounding boxes\. 

### Element Hierarchy<a name="bounding-box-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#bounding-box-regions-full-instructions), [short\-instructions](#bounding-box-regions-short-instructions)

### Regions<a name="bounding-box-regions"></a>

The following regions are required by this element\.

#### full\-instructions<a name="bounding-box-regions-full-instructions"></a>

General instructions about how to draw bounding boxes\.

#### short\-instructions<a name="bounding-box-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Output<a name="bounding-box-output"></a>

The following output is supported by this element\.

#### boundingBoxes<a name="bounding-box-output-boundingBoxes"></a>

An array of JSON objects, each of which specifies a bounding box that has been created by the worker\. Each JSON object in the array contains the following properties\.
+ **height** – The height of the box in pixels\.
+ **label** – The text assigned to the box as part of the labeling task\. This text must match one of the labels defined in the *labels* attribute of the <crowd\-bounding\-box> element\.
+ **left** – Distance of the top\-left corner of the box from the left side of the image, measured in pixels\.
+ **top** – Distance of the top\-left corner of the box from the top of the image, measured in pixels\.
+ **width** – The width of the box in pixels\.

#### inputImageProperties<a name="bounding-box-output-inputImageProperties"></a>

A JSON object that specifies the dimensions of the image that is being annotated by the worker\. This object contains the following properties\.
+ **height** – The height, in pixels, of the image\.
+ **width** – The width, in pixels, of the image\.

**Example : Sample Element Outputs**  
The following are samples of outputs from common use scenarios for this element\.  
**Single Label, Single Box / Multiple Label, Single Box**  

```
[
  {
    "annotatedResult": {
      "boundingBoxes": [
        {
          "height": 401,
          "label": "Dog",
          "left": 243,
          "top": 117,
          "width": 187
        }
      ],
      "inputImageProperties": {
        "height": 533,
        "width": 800
      }
    }
  }
]
```
**Single Label, Multiple Box**  

```
[
  {
    "annotatedResult": {
      "boundingBoxes": [
        {
          "height": 401,
          "label": "Dog",
          "left": 243,
          "top": 117,
          "width": 187
        },
        {
          "height": 283,
          "label": "Dog",
          "left": 684,
          "top": 120,
          "width": 116
        }
      ],
      "inputImageProperties": {
        "height": 533,
        "width": 800
      }
    }
  }
]
```
**Multiple Label, Multiple Box**  

```
[
  {
    "annotatedResult": {
      "boundingBoxes": [
        {
          "height": 395,
          "label": "Dog",
          "left": 241,
          "top": 125,
          "width": 158
        },
        {
          "height": 298,
          "label": "Cat",
          "left": 699,
          "top": 116,
          "width": 101
        }
      ],
      "inputImageProperties": {
        "height": 533,
        "width": 800
      }
    }
  }
]
```
You could have many labels available, but only the ones that are used appear in the output\.

### See Also<a name="bounding-box-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)