# crowd\-line<a name="sms-ui-template-crowd-line"></a>

A widget for drawing lines on an image\. Each line is associated with a label, and output data will report the starting and ending points of each line\. 

The following is an example of a Liquid template that uses the `<crowd-line>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. For more examples, see this [GitHub repository](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/tree/master/images)\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-line
    name="crowdLine"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="Add header here to describe the task"
    labels="['car','pedestrian','street car']"
  >
    <short-instructions>
        <p>Read the task carefully and inspect the image.</p>
        <p>Choose the appropriate label that best suits the image.</p>
        <p>Draw a line on each objects that the label applies to.</p>
    </short-instructions>

    <full-instructions>
        <p>Read the task carefully and inspect the image.</p>
        <p>Choose the appropriate label that best suits the image. 
        <p>Draw a line along each object that the image applies to.
            Make sure that the line does not extend beyond the boundaries
            of the object.
        </p>
        <p>Each line is defined by a starting and ending point. Carefully
        place the starting and ending points on the boundaries of the object.</p>
    </full-instructions>
    
  </crowd-line>
</crowd-form>
```

### Attributes<a name="line-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="line-attributes-header"></a>

Optional\. The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### initial\-value<a name="line-attributes-initialValue"></a>

Optional\. An array of JSON objects, each of which sets a line when the component is loaded\. Each JSON object in the array contains the following properties:
+ **label** – The text assigned to the line as part of the labeling task\. This text must match one of the labels defined in the *labels* attribute of the `<crowd-line>` element\.
+ **vertices** – the `x` and `y` pixel corrdinates of the start point and end point of the line, relative to the top\-left corner of the image\.

```
initial-value="{
    lines: [
    {
        label: 'sideline', // label of this line annotation
        vertices:[         // an array of vertices which decide the position of the line
        {
            x: 84,
            y: 110
        },
        {
            x: 60,
            y: 100
        }
        ]
    },
    {
        label: 'yardline',
        vertices:[       
        {
            x: 651,
            y: 498
        },
        {
            x: 862,
            y: 869
        }
        ]
    }
   ]
}"
```

Lines set via the `initial-value` property can be adjusted\. Whether or not a worker answer was adjusted is tracked via an `initialValueModified` boolean in the worker answer output\.

#### labels<a name="line-attributes-labels"></a>

Required\. A JSON formatted array of strings, each of which is a label that a worker can assign to the line\. 

**Limit:** 10 labels

#### label\-colors<a name="line-attributes-label-colors"></a>

Optional\. An array of strings\. Each string is a hexadecimal \(hex\) code for a label\.

#### name<a name="line-attributes-name"></a>

Required\. The name of this widget\. It's used as a key for the widget's input in the form output\.

#### src<a name="line-attributes-src"></a>

Required\. The URL of the image on which to draw lines\. 

### Regions<a name="line-regions"></a>

The following regions are required by this element\.

#### full\-instructions<a name="line-regions-full-instructions"></a>

General instructions about how to draw lines\. 

#### short\-instructions<a name="line-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Element Hierarchy<a name="line-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [short\-instructions](#line-regions-short-instructions), [full\-instructions](#line-regions-full-instructions)

### Output<a name="line-output"></a>

#### inputImageProperties<a name="line-output-inputImageProperties"></a>

A JSON object that specifies the dimensions of the image that is being annotated by the worker\. This object contains the following properties\.
+ **height** – The height, in pixels, of the image\.
+ **width** – The width, in pixels, of the image\.

#### lines<a name="line-output-labelMappings"></a>

A JSON Array containing objects with the line labels and vertices\.
+ **label** – The label given to a line\.
+ vertices – the `x` and `y` pixel corrdinates of the start point and end point of the line, relative to the top\-left corner of the image\.

**Example : Sample Element Outputs**  
The following is an example of output from this element\.

```
{
    "crowdLine": { //This is the name you set for the crowd-line
      "inputImageProperties": {
        "height": 1254,
        "width": 2048
      },
      "lines": [
        {
          "label": "yardline",
          "vertices": [ 
            {
              "x": 58,
              "y": 295
            },
            {
              "x": 1342,
              "y": 398
            }
          ]
        },
        {
          "label": "sideline",
          "vertices": [
            {
              "x": 472,
              "y": 910
            },
            {
              "x": 1480,
              "y": 600
            }
          ]
        }
      ]
    }
  }
```

### See Also<a name="line-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)