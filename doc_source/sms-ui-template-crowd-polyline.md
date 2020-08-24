# crowd\-polyline<a name="sms-ui-template-crowd-polyline"></a>

A widget for drawing polylines or lines on an image\. Each polyline is associated with a label and can include two or more vertices\. A polyline can intersect itself and its starting and ending points can be placed anywhere on the image\.

The following is an example of a Liquid template that uses the `<crowd-polyline>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. For more examples, see this [GitHub repository](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/tree/master/images)\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-polyline
    name="crowdPolyline"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="Add header here to describe the task"
    labels="['car','pedestrian','street car']"
  >
    <full-instructions>
        <p>Read the task carefully and inspect the image.</p>
        <p>Choose the appropriate label that best suits the image.</p>
        <p>Draw a polyline around the boundaries of all objects
        that the label applies to.</p>
        <p>Use the <b>Enter</b> key to complete a polyline.</p>
        <p>Make sure that the polyline fits tightly around the boundary
        of the object.</p>
    </full-instructions>

    <short-instructions>
        <p>Read the task carefully and inspect the image.</p>
        <p>Review the tool guide to learn how to use the polyline tool.</p>
        <p>Choose the appropriate label that best suits the image.</p>
        <p>To draw a polyline, select a label that applies to an object of interest 
            and add a single point to the photo by clicking on that point. Continue to 
            draw the polyline around the object by adding additional points
            around the object boundary.</p>
        <p>After you place the final point on the polyline, press <b>Enter</b> on your
        keyboard to complete the polyline.</p>

    </short-instructions>
  </crowd-polyline>
</crowd-form>
```

### Attributes<a name="polyline-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="polyline-attributes-header"></a>

Optional\. The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### initial\-value<a name="polyline-attributes-initialValue"></a>

Optional\. An array of JSON objects, each of which sets a polyline when the component is loaded\. Each JSON object in the array contains the following properties:
+ **label** – The text assigned to the polyline as part of the labeling task\. This text must match one of the labels defined in the *labels* attribute of the `<crowd-polyline>` element\.
+ **vertices** – the `x` and `y` pixel corrdinates of the vertices of a polyline, relative to the top\-left corner of the image\.

```
 initial-value= "{
    polylines: [
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
        },
        {
            x: 1000,
            y: 869
        }
        ]
    }
   ]
}"
```

Polylines set via the `initial-value` property can be adjusted\. Whether or not a worker answer was adjusted is tracked via an `initialValueModified` boolean in the worker answer output\.

#### labels<a name="polyline-attributes-labels"></a>

Required\. A JSON formatted array of strings, each of which is a label that a worker can assign to the line\. 

**Limit:** 10 labels

#### label\-colors<a name="polyline-attributes-label-colors"></a>

Optional\. An array of strings\. Each string is a hexadecimal \(hex\) code for a label\.

#### name<a name="polyline-attributes-name"></a>

Required\. The name of this widget\. It's used as a key for the widget's input in the form output\.

#### src<a name="polyline-attributes-src"></a>

Required\. The URL of the image on which to draw polylines\.

### Regions<a name="polyline-regions"></a>

The following regions are required by this element\.

#### full\-instructions<a name="polyline-regions-full-instructions"></a>

General instructions about how to draw polylines\. 

#### short\-instructions<a name="polyline-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Element Hierarchy<a name="polyline-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [short\-instructions](#polyline-regions-short-instructions), [full\-instructions](#polyline-regions-full-instructions)

### Output<a name="polyline-output"></a>

#### inputImageProperties<a name="polyline-output-inputImageProperties"></a>

A JSON object that specifies the dimensions of the image that is being annotated by the worker\. This object contains the following properties\.
+ **height** – The height, in pixels, of the image\.
+ **width** – The width, in pixels, of the image\.

#### polylines<a name="polyline-output-labelMappings"></a>

A JSON Array containing objects with polylines' labels and vertices\.
+ **label** – The label given to a line\.
+ vertices – the `x` and `y` pixel corrdinates of the vertices of a polyline, relative to the top\-left corner of the image\.

**Example : Sample Element Outputs**  
The following is an example of output from this element\.

```
 {
    "crowdPolyline": { //This is the name you set for the crowd-polyline
      "inputImageProperties": {
        "height": 1254,
        "width": 2048
      },
      "polylines": [
        {
          "label": "sideline",
          "vertices": [
            {
              "x": 651,
              "y": 498
            },
            {
              "x": 862,
              "y": 869
            },
            {
              "x": 1449,
              "y": 611
            }
          ]
        },
        {
          "label": "yardline",
          "vertices": [
            {
              "x": 1148,
              "y": 322
            },
            {
              "x": 1705,
              "y": 474
            },
            ,
            {
              "x": 1755,
              "y": 474
            }
          ]
        }
      ]
    }
  }
```

### See Also<a name="polyline-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)