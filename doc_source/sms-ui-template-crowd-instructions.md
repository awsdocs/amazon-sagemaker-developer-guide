# crowd\-instructions<a name="sms-ui-template-crowd-instructions"></a>

An element that displays instructions on three tabbed pages, **Summary**, **Detailed Instructions**, and **Examples**, when the worker clicks on a link or button\.

The following is an example of a Liquid template that used the `<crowd-instructions>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  	<crowd-instructions link-text="View instructions" link-type="button">
		  <short-summary>
		    <p>Given an image, write three words or short phrases that summarize its contents.</p>
		  </short-summary>
		  <detailed-instructions>
		    <p>Imagine that you are describing an image to a friend or tagging it for a news website. Provide three specific words or short phrases that describe it.</p>
		  </detailed-instructions>
		  <positive-example>
		    <p><img src="https://s3.amazonaws.com/cv-demo-images/highway.jpg"/></p>
		    <p>
		    	<ul>
		    		<li>Highway</li>
		    		<li>Cars</li>
		    		<li>Gas station</li>
		    	</ul>
		    </p>
		  </positive-example>
		  <negative-example>
		    <p><img src="https://s3.amazonaws.com/cv-demo-images/highway.jpg"/></p>
		    <p>
		    	These are not specific enough:
		    	<ol>
		    		<li>Trees</li>
		    		<li>Outside</li>
		    		<li>Daytime</li>
		    	</ol>
		    </p>
		  </negative-example>
	</crowd-instructions>
    <p><strong>Instructions: </strong>Given an image, write three words or short phrases that summarize its contents.</p>
    <p>If someone were to see these three words or phrases, they should understand the subject and context of the image, as well as any important actions.</p>
	<p>View the instructions for detailed instructions and examples.</p>
	<p><img style="max-width: 100%; max-height: 100%" src="{{ task.input.taskObject | grant_read_access }}"></p>
  <crowd-input name="tag1" label="Word/phrase 1" required></crowd-input>
  <crowd-input name="tag2" label="Word/phrase 2" required></crowd-input>
  <crowd-input name="tag3" label="Word/phrase 3" required></crowd-input>
</crowd-form>
```

### Attributes<a name="instructions-attributes"></a>

The following attributes are supported by this element\.

#### link\-text<a name="instructions-attributes-link-text"></a>

The text to display for opening the instructions\. The default is **Click for instructions**\.

#### link\-type<a name="instructions-attributes-link-type"></a>

A string that specifies the type of trigger for the instructions\. The possible values are "link" \(default\) and "button"\.

### Element Hierarchy<a name="instructions-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### Regions<a name="instructions-regions"></a>

The following regions are supported by this element\.

#### detailed\-instructions<a name="instructions-regions-detailed-instructions"></a>

Content that provides specific instructions for a task\. This appears on the page of the "Detailed Instructions" tab\.

#### negative\-example<a name="instructions-regions-negative-examples"></a>

Content that provides examples of inadequate task completion\. This appears on the page of the "Examples" tab\. More than one example may be provided within this element\.

#### positive\-example<a name="instructions-regions-positive-examples"></a>

Content that provides examples of proper task completion\. This appears on the page of the "Examples" tab\.

#### short\-summary<a name="instructions-regions-short-summary"></a>

A brief statement that summarizes the task to be completed\. This appears on the page of the "Summary" tab\. More than one example may be provided within this element\.

### See Also<a name="instructions-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)