# crowd\-instance\-segmentation<a name="sms-ui-template-crowd-instance-segmentation"></a>

A widget for identifying individual instances of specific objects within an image and creating a colored overlay for each labeled instance\.

The following is an example of a Liquid template that uses the `<crowd-instance-segmentation>`\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-instance-segmentation
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
      <p>Use the tools to label all instances of the requested items in the image</p>
    </short-instructions>
  </crowd-instance-segmentation>
</crowd-form>
```

Use a template similar to the following to allow workers to add their own categories \(labels\)\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-instance-segmentation
    id="annotator"
    name="myTexts"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="Click Instructions to add new labels."
    labels="['placeholder']"
  >
    <short-instructions>
      <h3>Add a label to describe each type of object in this image.</h3>
      <h3>Cover each instance of each object with a segmentation mask.</h3>
      <br>
      <h3>
        Add new label
      </h3>
      <crowd-input name="_customLabel" id="customLabel"></crowd-input>
      <crowd-button id="addLabel">Add</crowd-button>
      
      <br><br><br>
      <h3>
      Manage labels
      </h3>
      <div id="labelsSection"></div>
    </short-instructions>
    
    <full-instructions>
      Describe your task in more detail here.
    </full-instructions>
  </crowd-instance-segmentation>
</crowd-form>

<script>
  document.addEventListener('all-crowd-elements-ready', function(event) {
    document.querySelector('crowd-instance-segmentation').labels = [];  
  });
  
  function populateLabelsSection() {
    labelsSection.innerHTML = '';
    annotator.labels.forEach(function(label) {
      const labelContainer = document.createElement('div');
      labelContainer.innerHTML = label + ' <a href="javascript:void(0)">(Delete)</a>';
      labelContainer.querySelector('a').onclick = function() {
        annotator.labels = annotator.labels.filter(function(l) {
          return l !== label;
        });
        populateLabelsSection();
      };
      labelsSection.appendChild(labelContainer);
    });
  }

  addLabel.onclick = function() {
    annotator.labels = annotator.labels.concat([customLabel.value]);
    customLabel.value = null;
    
    populateLabelsSection();
  };
</script>
```

### Attributes<a name="instance-segmentation-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="instance-segmentation-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### labels<a name="instance-segmentation-attributes-labels"></a>

A JSON formatted array of strings, each of which is a label that a worker can assign to an instance of an object in the image\. Workers can generate different overlay colors for each relevant instance by selecting "add instance" under the label in the tool\.

#### name<a name="instance-segmentation-attributes-name"></a>

The name of this widget\. It is used as a key for the labeling data in the form output\.

#### src<a name="instance-segmentation-attributes-src"></a>

The URL of the image that is to be labeled\.

### initial\-value<a name="instance-segmentation-attributes-initial-value"></a>

A JSON object containing the color mappings of a prior instance segmentation job and a link to the overlay image output by the prior job\. Include this when you want a human worker to verify the results of a prior labeling job and adjust it if necessary\.

The attribute will appear as follows:

```
  initial-value="{
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
    "src": {{ "S3 file URL for image" | grant_read_access }}
  }"
```

### Element Hierarchy<a name="instance-segmentation-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#instance-segmentation-regions-full-instructions), [short\-instructions](#instance-segmentation-regions-short-instructions)

### Regions<a name="instance-segmentation-regions"></a>

The following regions are supported by this element\.

#### full\-instructions<a name="instance-segmentation-regions-full-instructions"></a>

General instructions about how to do image segmentation\.

#### short\-instructions<a name="instance-segmentation-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Output<a name="instance-segmentation-output"></a>

The following output is supported by this element\.

#### labeledImage<a name="instance-segmentation-output-labeledImage"></a>

A JSON Object containing a Base64 encoded PNG of the labels\.

#### instances<a name="instance-segmentation-output-labelMappings"></a>

A JSON Array containing objects with the instance labels and colors\.
+ **color** – The hexadecimal value of the label's RGB color in the `labeledImage` PNG\.
+ **label** – The label given to overlay\(s\) using that color\. This value may repeat, because the different instances of the label are identified by their unique color\.

#### inputImageProperties<a name="instance-segmentation-output-inputImageProperties"></a>

A JSON object that specifies the dimensions of the image that is being annotated by the worker\. This object contains the following properties\.
+ **height** – The height, in pixels, of the image\.
+ **width** – The width, in pixels, of the image\.

**Example : Sample Element Outputs**  
The following is an example of output from this element\.  

```
[
  {
    "annotatedResult": {
      "inputImageProperties": {
        "height": 533,
        "width": 800
      },
      "instances": [
        {
          "color": "#1f77b4",
          "label": "<Label 1>": 
        },
        {
          "color": "#2ca02c",
          "label": "<Label 1>": 
        },
        {
          "color": "#ff7f0e",
          "label": "<Label 3>": 
        },
      ],
      "labeledImage": {
        "pngImageData": "<Base-64 Encoded Data>"
      }
    }
  }
]
```

### See Also<a name="instance-segmentation-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)