# Step 2: Creating your custom labeling task template<a name="sms-custom-templates-step2"></a>

**Topics**
+ [Starting with a base template](#sms-custom-templates-step2-base)
+ [Developing templates locally](#sms-custom-template-step2-UI-local)
+ [Using External Assets](#sms-custom-template-step2-UI-external)
+ [Track your variables](#sms-custom-template-step2-UI-vars)
+ [A simple sample](#sms-custom-templates-step2-sample)
+ [Adding automation with Liquid](#sms-custom-templates-step2-automate)
+ [End\-to\-end demos](#sms-custom-templates-step2-moredemos)
+ [Next](#templates-step2-next)

## Starting with a base template<a name="sms-custom-templates-step2-base"></a>

To get you started, the **Task type** starts with a drop\-down menu listing a number of our more common task types, plus a custom type\. Choose one and the code editor area will be filled with a sample template for that task type\. If you prefer not to start with a sample, choose **Custom HTML** for a minimal template skeleton\.

If you've already created a template, upload the file directly using the **Upload file** button in the upper right of the task setup area or paste your template code into the editor area\. For a repository of demo templates for a variety of labeling job task types, see [Amazon SageMaker Ground Truth Sample Task UIs ](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis)\.

## Developing templates locally<a name="sms-custom-template-step2-UI-local"></a>

While you need to be in the console to test how your template will process incoming data, you can test the look and feel of your template's HTML and custom elements in your browser by adding this code to the top of your HTML file\.

**Example**  

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
```

This loads the necessary code to render the custom HTML elements\. Use this if you want to develop your template's look and feel in your preferred editor rather than in the console\.

Remember, though, this will not parse your variables\. You may want to replace them with sample content while developing locally\.

## Using External Assets<a name="sms-custom-template-step2-UI-external"></a>

Amazon SageMaker Ground Truth custom templates allow external scripts and style sheets to be embedded\.

**Example**  

```
<script src="https://www.example.com/my-enhancment-script.js"></script>
<link rel="stylesheet" type="text/css" href="https://www.example.com/my-enhancement-styles.css">
```

If you encounter errors, ensure that your originating server is sending the correct MIME type and encoding headers with the assets\.

For example, the MIME and encoding types for remote scripts: `application/javascript;CHARSET=UTF-8`\.

The MIME and encoding type for remote stylesheets: `text/css;CHARSET=UTF-8`\.

## Track your variables<a name="sms-custom-template-step2-UI-vars"></a>

In the process of building the sample below, there will be a step that adds variables to it to represent the pieces of data that may change from task to task, worker to worker\. If you're starting with one of the sample templates, you will need to make sure you're aware of the variables it already uses\. When you create your pre\-annotation AWS Lambda script, its output will need to contain values for any of those variables you choose to keep\.

The values you use for the variables can come from your manifest file\. All the key\-value pairs in your data object are provided to your pre\-annotation Lambda\. If it's a simple pass\-through script, matching keys for values in your data object to variable names in your template is the easiest way to pass those values through to the tasks forms your workers see\.

## A simple sample<a name="sms-custom-templates-step2-sample"></a>

 All tasks begin and end with the `<crowd-form> </crowd-form>` elements\. Like standard HTML `<form>` elements, all of your form code should go between them\. 

 For a simple tweet\-analysis task, use the `<crowd-classifier>` element\. It requires the following attributes: 
+ *name* \- the variable name to use for the result in the form output\.
+ *categories* \- a JSON formatted array of the possible answers\.
+ *header* \- a title for the annotation tool

As children of the `<crowd-classifier>` element, you must have three regions\.
+ *<classification\-target>* \- the text the worker will classify based on the options specified in the `categories` attribute above\.
+ *<full\-instructions>* \- instructions that are available from the "View full instructions" link in the tool\. This can be left blank, but it is recommended that you give good instructions to get better results\.
+ *<short\-instructions>* \- a more brief description of the task that appears in the tool's sidebar\. This can be left blank, but it is recommended that you give good instructions to get better results\.

A simple version of this tool would look like this\.

**Example of using `crowd-classifier`**  

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-classifier 
    name="tweetFeeling"
    categories="['positive','negative','neutral', 'unclear']"
    header="Which term best describes this tweet?" 
  >
     <classification-target>
      My favorite football team won today! 
      Bring on the division finals!
    </classification-target>

    <full-instructions header="Sentiment Analysis Instructions">
      Try to determine the sentiment the author 
      of the tweet is trying to express. 
      If none seem to match, choose "cannot determine."
    </full-instructions>

    <short-instructions>
      Pick the term best describing the sentiment 
      of the tweet. 
    </short-instructions>

  </crowd-classifier>
</crowd-form>
```

You can copy and paste the code into the editor in the Ground Truth labeling job creation workflow to preview the tool, or try out a [demo of this code on CodePen\.](https://codepen.io/MTGT/full/OqBvJw)

 [ ![\[View a demo of this sample template on CodePen\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pen.gif) ](https://codepen.io/MTGT/full/OqBvJw) 

## Adding automation with Liquid<a name="sms-custom-templates-step2-automate"></a>

Our custom template system uses [Liquid](https://shopify.github.io/liquid/) for automation\. It is an open source inline markup language\. For more information and documentation, visit the [Liquid homepage](https://shopify.github.io/liquid/)\.

The most common use of Liquid will be to parse the data coming from your **pre\-annotation Lambda** and pull out the relevant variables to create the task\. In Liquid, the text between single curly braces and percent symbols is an instruction or "tag" that creates control flow\. Text between double curly braces is a variable or "object" which outputs its value\.

The `taskInput` object returned by your [Pre\-annotation Lambda](sms-custom-templates-step3.md#sms-custom-templates-step3-prelambda) will be available as the `task.input` object in your templates\.

The properties in your manifest's data objects are passed into your [Pre\-annotation Lambda](sms-custom-templates-step3.md#sms-custom-templates-step3-prelambda) as the `event.dataObject`\. A simple pass\-through script simply returns that object as the `taskInput` object\. You would represent values from your manifest as variables as follows\.

**Example Manifest data object**  

```
{
  "source": "This is a sample text for classification",
  "labels": [ "angry" , "sad" , "happy" , "inconclusive" ],
  "header": "What emotion is the speaker feeling?"
}
```

**Example Sample HTML using variables**  

```
<crowd-classifier 
  name='tweetFeeling'
  categories='{{ task.input.labels | to_json }}'
  header='{{ task.input.header }}' >
<classification-target>
  {{ task.input.source }}
</classification-target>
```

Note the addition of "` | to_json`" to the `labels` property above\. That's a filter to turn the array into a JSON representation of the array\. Variable filters are explained next\.

### Variable filters<a name="sms-custom-templates-step2-automate-filters"></a>

In addition to the standard Liquid filters and actions, Ground Truth offers a few additional filters\. Filters are applied by placing a pipe \(`|`\) character after the variable name, then specifying a filter name\. Filters can be chained in the form of:

**Example**  

```
{{ <content> | <filter> | <filter> }}
```

#### Autoescape and explicit escape<a name="sms-custom-templates-step2-automate-filters-autoescape"></a>

By default, inputs will be HTML escaped to prevent confusion between your variable text and HTML\. You can explicitly add the `escape` filter to make it more obvious to someone reading the source of your template that the escaping is being done\.

#### escape\_once<a name="sms-custom-templates-step2-automate-escapeonce"></a>

`escape_once` ensures that if you've already escaped your code, it doesn't get re\-escaped on top of that\. For example, so that &amp; doesn't become &amp;amp;\.

#### skip\_autoescape<a name="sms-custom-templates-step2-automate-skipautoescape"></a>

`skip_autoescape` is useful when your content is meant to be used as HTML\. For example, you might have a few paragraphs of text and some images in the full instructions for a bounding box\.

**Use `skip_autoescape` sparingly**  
The best practice in templates is to avoid passing in functional code or markup with `skip_autoescape` unless you are absolutely sure you have strict control over what's being passed\. If you're passing user input, you could be opening your workers up to a Cross Site Scripting attack\.

#### to\_json<a name="sms-custom-templates-step2-automate-tojson"></a>

`to_json` will encode what you feed it to JSON \(JavaScript Object Notation\)\. If you feed it an object, it will serialize it\.

#### grant\_read\_access<a name="sms-custom-templates-step2-automate-grantreadaccess"></a>

`grant_read_access` takes an S3 URI and encodes it into an HTTPS URL with a short\-lived access token for that resource\. This makes it possible to display to workers photo, audio, or video objects stored in S3 buckets that are not otherwise publicly accessible\.

**Example of the filters**  
Input  

```
auto-escape: {{ "Have you read 'James & the Giant Peach'?" }}
explicit escape: {{ "Have you read 'James & the Giant Peach'?" | escape }}
explicit escape_once: {{ "Have you read 'James &amp; the Giant Peach'?" | escape_once }}
skip_autoescape: {{ "Have you read 'James & the Giant Peach'?" | skip_autoescape }}
to_json: {{ jsObject | to_json }}                
grant_read_access: {{ "s3://mybucket/myphoto.png" | grant_read_access }}
```

**Example**  
Output  

```
auto-escape: Have you read &#39;James &amp; the Giant Peach&#39;?
explicit escape: Have you read &#39;James &amp; the Giant Peach&#39;?
explicit escape_once: Have you read &#39;James &amp; the Giant Peach&#39;?
skip_autoescape: Have you read 'James & the Giant Peach'?
to_json: { "point_number": 8, "coords": [ 59, 76 ] }
grant_read_access: https://s3.amazonaws.com/mybucket/myphoto.png?<access token and other params>
```

**Example of an automated classification template\.**  
To automate the simple text classification sample, replace the tweet text with a variable\.  
The text classification template is below with automation added\. The changes/additions are highlighted in bold\.  

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-classifier 
    name="tweetFeeling"
    categories="['positive', 'negative', 'neutral', 'cannot determine']"
    header="Which term best describes this tweet?" 
  >
    <classification-target>
       {{ task.input.source }}
    </classification-target>

    <full-instructions header="Analyzing a sentiment">
      Try to determine the feeling the author 
      of the tweet is trying to express. 
      If none seem to match, choose "other."
    </full-instructions>

    <short-instructions>
      Pick the term best describing the sentiment 
      of the tweet. 
    </short-instructions>

  </crowd-classifier>
</crowd-form>
```
The tweet text that was in the prior sample is now replaced with an object\. The `entry.taskInput` object uses `source` \(or another name you specify in your pre\-annotation Lambda\) as the property name for the text and it is inserted directly in the HTML by virtue of being between double curly braces\.

## End\-to\-end demos<a name="sms-custom-templates-step2-moredemos"></a>

You can view the following end\-to\-end demos which include sample Lambdas:
+ [Demo Template: Annotation of Images with `crowd-bounding-box`](sms-custom-templates-step2-demo1.md)
+ [Demo Template: Labeling Intents with `crowd-classifier`](sms-custom-templates-step2-demo2.md)

## Next<a name="templates-step2-next"></a>

[Step 3: Processing with AWS Lambda](sms-custom-templates-step3.md)