# crowd\-instructions<a name="sms-ui-template-crowd-instructions"></a>

An element that displays instructions on three tabbed pages, **Summary**, **Detailed Instructions**, and **Examples**, when the worker clicks on a link or button\.

## Attributes<a name="instructions-attributes"></a>

The following attributes are supported by this element\.

### link\-text<a name="instructions-attributes-link-text"></a>

The text to display for opening the instructions\. The default is **Click for instructions**\.

### link\-type<a name="instructions-attributes-link-type"></a>

A string that specifies the type of trigger for the instructions\. The possible values are "link" \(default\) and "button"\.

## Element Hierarchy<a name="instructions-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

## Regions<a name="instructions-regions"></a>

The following regions are supported by this element\.

### detailed\-instructions<a name="instructions-regions-detailed-instructions"></a>

Content that provides specific instructions for a task\. This appears on the page of the "Detailed Instructions" tab\.

### negative\-example<a name="instructions-regions-negative-examples"></a>

Content that provides examples of inadequate task completion\. This appears on the page of the "Examples" tab\. More than one example may be provided within this element\.

### positive\-example<a name="instructions-regions-positive-examples"></a>

Content that provides examples of proper task completion\. This appears on the page of the "Examples" tab\.

### short\-summary<a name="instructions-regions-short-summary"></a>

A brief statement that summarizes the task to be completed\. This appears on the page of the "Summary" tab\. More than one example may be provided within this element\.

## See Also<a name="instructions-see-also"></a>

For more information, see the following\.
+ [Amazon SageMaker Ground Truth](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)