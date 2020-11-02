# crowd\-slider<a name="sms-ui-template-crowd-slider"></a>

A bar with a sliding knob that allows a worker to select a value from a range of values by moving the knob\. The slider makes it a great choice for settings that reflect intensity levels, such as volume, brightness, or color saturation\.

The following is an example of a survey template that uses the `<crowd-slider>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
<crowd-instructions link-text="View instructions" link-type="button">
  <short-summary>
    <p>Provide a brief instruction here</p>
  </short-summary>

  <detailed-instructions>
    <h3>Provide more detailed instructions here</h3>
    <p>Include additional information</p>
  </detailed-instructions>

  <positive-example>
    <p>Provide an example of a good answer here</p>
    <p>Explain why it's a good answer</p>
  </positive-example>

  <negative-example>
    <p>Provide an example of a bad answer here</p>
    <p>Explain why it's a bad answer</p>
  </negative-example>
</crowd-instructions>

<div>
  <p>What is your favorite color for a bird?</p>
  <crowd-input name="favoriteColor" placeholder="example: pink" required></crowd-input>
</div>

<div>
  <p>Check this box if you like birds</p>
  <crowd-checkbox name="likeBirds" checked="true" required></crowd-checkbox>
</div>

<div>
  <p>On a scale of 1-10, how much do you like birds?</p>
  <crowd-slider name="howMuch" min="1" max="10" step="1" pin="true" required></crowd-slider>
</div>

<div>
  <p>Write a short essay describing your favorite bird</p>
  <crowd-text-area name="essay" rows="4" placeholder="Lorem ipsum..." required></crowd-text-area>
</div>
</crowd-form>
```

### Attributes<a name="slider-attributes"></a>

The following attributes are supported by this element\.

#### disabled<a name="slider-attributes-disabled"></a>

A Boolean switch that, if present, displays the slider as disabled\.

#### editable<a name="slider-attributes-editable"></a>

A Boolean switch that, if present, displays an up/down button that can be chosen to select the value\.

Selecting the value via the up/down button is an alternative to selecting the value by moving the knob on the slider\. The knob on the slider will move synchronously with the up/down button choices\.

#### max<a name="slider-attributes-max"></a>

A number that specifies the maximum value on the slider\.

#### min<a name="slider-attributes-min"></a>

A number that specifies the minimum value on the slider\.

#### name<a name="slider-attributes-name"></a>

A string that is used to identify the answer submitted by the worker\. This value will match a key in the JSON object that specifies the answer\.

#### pin<a name="slider-attributes-pin"></a>

A Boolean switch that, if present, displays the current value above the knob as the knob is moved\.

#### required<a name="slider-attributes-required"></a>

A Boolean switch that, if present, requires the worker to provide input\.

#### secondary\-progress<a name="slider-attributes-secondary-progress"></a>

When used with a `crowd-slider-secondary-color` CSS attribute, the progress bar is colored to the point represented by the `secondary-progress`\. For example, if this was representing the progress on a streaming video, the `value` would represent where the viewer was in the video timeline\. The `secondary-progress` value would represent the point on the timeline to which the video had buffered\.

#### step<a name="slider-attributes-step"></a>

A number that specifies the difference between selectable values on the slider\.

#### value<a name="slider-attributes-value"></a>

A preset that becomes the default if the worker does not provide input\.

### Element Hierarchy<a name="slider-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### See Also<a name="slider-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)