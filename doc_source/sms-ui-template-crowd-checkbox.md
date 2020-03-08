# crowd\-checkbox<a name="sms-ui-template-crowd-checkbox"></a>

A UI component that can be checked or unchecked allowing a user to select multiple options from a set\.

### Attributes<a name="checkbox-attributes"></a>

The following attributes are supported by this element\.

#### checked<a name="checkbox-attributes-checked"></a>

A Boolean switch that, if present, displays the check box as checked\.

#### disabled<a name="checkbox-attributes-disabled"></a>

A Boolean switch that, if present, displays the check box as disabled and prevents it from being checked\.

#### name<a name="checkbox-attributes-name"></a>

A string that is used to identify the answer submitted by the worker\. This value will match a key in the JSON object that specifies the answer\.

#### required<a name="checkbox-attributes-required"></a>

A Boolean switch that, if present, requires the worker to provide input\.

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
<div><crowd-checkbox name="myformbit" value="Red"> Red </div>
<div><crowd-checkbox name="myformbit" value="Yellow"> Yellow </div>
<div><crowd-checkbox name="myformbit" value="Green"> Green </div>
```

```
//Output with "Red" checked
[
  {
    "myformbit": {
      "Green": false,
      "Red": true,
      "Yellow": false
    }
  }
]
```
Note that all three color values are properties of a single object\.  
**Using different `name` values for each box\.**  

```
<!-- INPUT -->
<div><crowd-checkbox name="Stop" value="Red"> Red </div>
<div><crowd-checkbox name="Slow" value="Yellow"> Yellow </div>
<div><crowd-checkbox name="Go" value="Green"> Green </div>
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
+ [Use Amazon SageMaker Ground Truth for Labeling](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)