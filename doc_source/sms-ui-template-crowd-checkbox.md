# crowd\-checkbox<a name="sms-ui-template-crowd-checkbox"></a>

A UI component that can be checked or unchecked allowing a user to select multiple options from a set\.

The following is an example of a Liquid template that uses the `<crowd-checkbox>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  
  <p>Find the official website for: <strong>{{ task.input.company }}</strong></p>
  <p>Do not give Yelp pages, LinkedIn pages, etc.</p>
  <p>Include the http:// prefix from the website</p>
  <crowd-input name="website" placeholder="http://example.com"></crowd-input>

  <crowd-checkbox name="website-found">Website Found</crowd-checkbox>

</crowd-form>
```

### Attributes<a name="checkbox-attributes"></a>

The following attributes are supported by this element\.

#### checked<a name="checkbox-attributes-checked"></a>

A Boolean switch that, if present, displays the check box as checked\.

The following is an example of the syntx used to check a checkbox by default\.

```
  <crowd-checkbox name="checkedBox" value="checked" checked>This box is checked</crowd-checkbox>
```

#### disabled<a name="checkbox-attributes-disabled"></a>

A Boolean switch that, if present, displays the check box as disabled and prevents it from being checked\.

The following is an example of the syntax used to disable a checkbox\. 

```
  <crowd-checkbox name="disabledCheckBox" value="Disabled" disabled>Cannot be selected</crowd-checkbox>
```

#### name<a name="checkbox-attributes-name"></a>

A string that is used to identify the answer submitted by the worker\. This value will match a key in the JSON object that specifies the answer\.

#### required<a name="checkbox-attributes-required"></a>

A Boolean switch that, if present, requires the worker to provide input\.

The following is an example of the syntax used to require a checkbox be selected\.

```
  <crowd-checkbox name="work_verified" required>Instructions were clear</crowd-checkbox>
```

#### value<a name="checkbox-attributes-value"></a>

A string used as the name for the check box state in the output\. Defaults to "on" if not specified\. 

### Element Hierarchy<a name="checkbox-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### Output<a name="checkbox-element-output"></a>

Provides a JSON object\. The `name` string is the object name and the `value`string is the property name for a Boolean value based on the check box state; true if checked, false if not checked\.

**Example : Sample Element Outputs**  
**Using the same `name` value for multiple boxes\.**  

```
<!-- INPUT -->  
<div><crowd-checkbox name="image_attributes" value="blurry"> Blurry </crowd-checkbox></div>
<div><crowd-checkbox name="image_attributes" value="dim"> Too Dim </crowd-checkbox></div>
<div><crowd-checkbox name="image_attributes" value="exposed"> Too Bright </crowd-checkbox></div>
```

```
//Output with "blurry" and "dim" checked
[
  {
    "image_attributes": {
      "blurry": true,
      "dim": true,
      "exposed": false
    }
  }
]
```
Note that all three color values are properties of a single object\.  
**Using different `name` values for each box\.**  

```
<!-- INPUT -->
<div><crowd-checkbox name="Stop" value="Red"> Red </crowd-checkbox></div>
<div><crowd-checkbox name="Slow" value="Yellow"> Yellow </crowd-checkbox></div>
<div><crowd-checkbox name="Go" value="Green"> Green </crowd-checkbox></div>
```

```
//Output with "Red" checked
[
  {
    "Go": {
      "Green": false
    },
    "Slow": {
      "Yellow": false
    },
    "Stop": {
      "Red": true
    }
  }
]
```

### See Also<a name="checkbox-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)