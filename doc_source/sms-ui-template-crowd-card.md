# crowd\-card<a name="sms-ui-template-crowd-card"></a>

A box with an elevated appearance for displaying information\.

The following is an example of a template designed for sentiment analysis tasks that uses the `<crowd-card>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<style>
  h3 {
    margin-top: 0;
  }
  
  crowd-card {
    width: 100%;
  }
  
  .card {
    margin: 10px;
  }
  
  .left {
    width: 70%;
    margin-right: 10px;
    display: inline-block;
    height: 200px;
  }
  
  .right {
    width: 20%;
    height: 200px;
    display: inline-block;
  }
</style>

<crowd-form>
  <short-instructions>
    Your short instructions here.
  </short-instructions>
  
  <full-instructions>
    Your full instructions here.
  </full-instructions>
  
  <div class="left">
    <h3>What sentiment does this text convey?</h3>
    <crowd-card>
      <div class="card">
        Nothing is great.
      </div>
    </crowd-card>
  </div>
  
  <div class="right">
    <h3>Select an option</h3>
    
    <select name="sentiment1" style="font-size: large" required>
      <option value="">(Please select)</option>
      <option>Negative</option>
      <option>Neutral</option>
      <option>Positive</option>
      <option>Text is empty</option>
    </select>
  </div>
  
  <div class="left">
    <h3>What sentiment does this text convey?</h3>
    <crowd-card>
      <div class="card">
        Everything is great!
      </div>
    </crowd-card>
  </div>
  
  <div class="right">
    <h3>Select an option</h3>
    
    <select name="sentiment2" style="font-size: large" required>
      <option value="">(Please select)</option>
      <option>Negative</option>
      <option>Neutral</option>
      <option>Positive</option>
      <option>Text is empty</option>
    </select>
  </div>
</crowd-form>
```

### Attributes<a name="card-attributes"></a>

The following attributes are supported by this element\.

#### heading<a name="card-attributes-heading"></a>

The text displayed at the top of the box\.

#### image<a name="card-attributes-image"></a>

A URL to an image to be displayed within the box\.

### Element Hierarchy<a name="card-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### See Also<a name="card-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)