# crowd\-alert<a name="sms-ui-template-crowd-alert"></a>

A message that alerts the worker to a current situation\.

The following is an example of a Liquid template that uses the `<crowd-alert>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <div id="errorBox"></div>
   
  <crowd-keypoint
    src="{{ task.input.taskObject | grant_read_access }}"
    labels="['Item A', 'Item B', 'Item C']"        
    header="Please locate the centers of each item."
    name="annotatedResult">
    <short-instructions>
      Describe your task briefly here and give examples
    </short-instructions>
    <full-instructions>
      Give additional instructions and good/bad examples here
    </full-instructions>   
  </crowd-keypoint>
</crowd-form>

<script>
  var num_obj = 1;

  document.querySelector('crowd-form').onsubmit = function(e) {
    const keypoints = document.querySelector('crowd-keypoint').value.keypoints || document.querySelector('crowd-keypoint')._submittableValue.keypoints;
    const labels = keypoints.map(function(p) {
      return p.label;
    });

    // 1. Make sure total number of keypoints is correct.
    var original_num_labels = document.getElementsByTagName("crowd-keypoint")[0].getAttribute("labels");

    original_num_labels = original_num_labels.substring(2, original_num_labels.length - 2).split("\",\"");
    var goalNumKeypoints = num_obj*original_num_labels.length;
    if (keypoints.length != goalNumKeypoints) {
      e.preventDefault();
      errorBox.innerHTML = '<crowd-alert type="error" dismissible>You must add all keypoint annotations and use each label only once.</crowd-alert>';
      errorBox.scrollIntoView();
      return;
    }

    // 2. Make sure all labels are unique.
    labelCounts = {};
    for (var i = 0; i < labels.length; i++) {
      if (!labelCounts[labels[i]]) {
        labelCounts[labels[i]] = 0;
      }
      labelCounts[labels[i]]++;
    }
    const goalNumSingleLabel = num_obj;

    const numLabels = Object.keys(labelCounts).length;

    Object.entries(labelCounts).forEach(entry => {
      if (entry[1] != goalNumSingleLabel) {
        e.preventDefault();
        errorBox.innerHTML = '<crowd-alert type="error" dismissible>You must use each label only once.</crowd-alert>';
        errorBox.scrollIntoView();
      }
    })
  };
</script>
```

### Attributes<a name="alert-attributes"></a>

The following attributes are supported by this element\.

#### dismissible<a name="alert-attributes-dismissible"></a>

A Boolean switch that, if present, allows the message to be closed by the worker\.

#### type<a name="alert-attributes-type"></a>

A string that specifies the type of message to be displayed\. The possible values are "info" \(the default\), "success", "error", and "warning"\.

### Element Hierarchy<a name="alert-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### See Also<a name="alert-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)