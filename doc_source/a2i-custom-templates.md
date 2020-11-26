# Create Custom Worker Task Template<a name="a2i-custom-templates"></a>

*Crowd HTML Elements* are web components that provide a number of task widgets and design elements that you can tailor to the question you want to ask\. You can use these crowd elements to create a custom worker template and integrate it with an Amazon Augmented AI \(Amazon A2I\) human review workflow to customize the worker console and instructions\. 

For a list of all HTML crowd elements available to Amazon A2I users, see [Crowd HTML Elements Reference](sms-ui-template-reference.md)\. To see examples of templates, see the [AWS Github repository](https://github.com/aws-samples/amazon-a2i-sample-task-uis), which contains over 60 sample custom task templates\.

## Develop Templates Locally<a name="developing-templates-locally"></a>

When in the console to test how your template process incoming data, you can test the look and feel of your template's HTML and custom elements in your browser by adding the following code to the top of your HTML file\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
```

This loads the necessary code to render the custom HTML elements\. Use this code if you want to develop your template's look and feel in your preferred editor instead of in the console\.

This code won't parse your variables\. You might want to replace them with sample content while developing locally\.

## Use External Assets<a name="a2i-custom-template-using-external-assets"></a>

Amazon Augmented AI custom templates enable you to embed external scripts and style sheets\. For example, the following header embeds a `text/css` style sheet name `stylesheet` located at `https://www.example.com/my-enhancement-styles.css` into the custom template\.

**Example**  

```
<script src="https://www.example.com/my-enhancment-script.js"></script>
<link rel="stylesheet" type="text/css" href="https://www.example.com/my-enhancement-styles.css">
```

If you encounter errors, ensure that your originating server is sending the correct MIME type and encoding headers with the assets\.

For example, the MIME and encoding types for remote scripts is `application/javascript;CHARSET=UTF-8`\.

The MIME and encoding type for remote stylesheets is `text/css;CHARSET=UTF-8`\.

## Track Your Variables<a name="a2i-custom-template-step2-UI-vars"></a>

When building a custom template, you must add variables to it to represent the pieces of data that might change from task to task, or worker to worker\. If you're starting with one of the sample templates, you need to make sure you're aware of the variables it already uses\. 

For example, for a custom template that integrates an Augmented AI human review loop with a Amazon Textract text review task, `{{ task.input.selectedAiServiceResponse.blocks }}` is used for initial\-value input data\. For Amazon Augmented AI \(Amazon A2I\) integration with Amazon Rekognition , `{{ task.input.selectedAiServiceResponse.moderationLabels }}` is used\. For a custom task type, you need to determine the input parameter for your task type\. Use `{{ task.input.customInputValuesForStartHumanLoop}}` where you specify `customInputValuesForStartHumanLoop`\. 

## Custom Template Example for Amazon Textract<a name="a2i-custom-templates-textract-sample"></a>

All custom templates begin and end with the `<crowd-form> </crowd-form>` elements\. Like standard HTML `<form>` elements, all of your form code should go between these elements\. 

 For a Amazon Textract document analysis task, use the `<crowd-textract-document-analysis>` element\. It uses the following attributes: 
+ `src` – Specifies the URL of the image file to be annotated\.
+ `initialValue` – Sets initial values for attributes found in the worker UI\.
+ `blockTypes` \(required\) – Determines the kind of analysis that the workers can do\. Only `KEY_VALUE_SET` is currently supported\. 
+ `keys` \(required\) – Specifies new keys and the associated text value that the worker can add\.
+ `no-key-edit` \(required\) – Prevents the workers from editing the keys of annotations passed through `initialValue`\.
+ `no-geometry-edit` – Prevents workers from editing the polygons of annotations passed through `initialValue`\.

For children of the `<crowd-textract-document-analysis>` element, you must have two regions\. You can use arbitrary HTML and CSS elements in these regions\. 
+ `<full-instructions>` – Instructions that are available from the **View full instructions** link in the tool\. You can leave this blank, but we recommend that you provide complete instructions to get better results\.
+ `<short-instructions>` – A brief description of the task that appears in the tool's sidebar\. You can leave this blank, but we recommend that you provide complete instructions to get better results\.

 An Amazon Textract template would look similar to the following\.

**Example**  

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
{% capture s3_arn %}http://s3.amazonaws.com/{{ task.input.aiServiceRequest.document.s3Object.bucket }}/{{ task.input.aiServiceRequest.document.s3Object.name }}{% endcapture %}

<crowd-form>
  <crowd-textract-analyze-document
    src="{{ s3_arn | grant_read_access }}"
    initial-value="{{ task.input.selectedAiServiceResponse.blocks }}"
    header="Review the key-value pairs listed on the right and correct them if they don't match the following document."
    no-key-edit
    no-geometry-edit
    keys="{{ task.input.humanLoopContext.importantFormKeys }}"
    block-types="['KEY_VALUE_SET']"
  >
    <short-instructions header="Instructions">
      <style>
        .instructions {
          white-space: pre-wrap;
        }
        .instructionsImage {
          display: inline-block;
          max-width: 100%;
        }
      </style>
      <p class='instructions'>Choose a key-value block to highlight the corresponding key-value pair in the document.

If it is a valid key-value pair, review the content for the value. If the content is incorrect, correct it.

The text of the value is incorrect, correct it.
<img class='instructionsImage' src="https://example-site/correct-value-text.png" />

A wrong value is identified, correct it.
<img class='instructionsImage' src="https://example-site/correct-value.png" />

If it is not a valid key-value relationship, choose No.
<img class='instructionsImage' src="https://example-site/not-a-key-value-pair.png" />

If you can’t find the key in the document, choose Key not found.
<img class='instructionsImage' src="https://example-site/key-is-not-found.png" />

If the content of a field is empty, choose Value is blank.
<img class='instructionsImage' src="https://example-site/value-is-blank.png" />

<b>Examples</b>
Key and value are often displayed next to or below to each other.

Key and value displayed in one line.
<img class='instructionsImage' src="https://example-site/sample-key-value-pair-1.png" />

Key and value displayed in two lines.
<img class='instructionsImage' src="https://example-site/sample-key-value-pair-2.png" />

If the content of the value has multiple lines, enter all the text without a line break. Include all value text even if it extends beyond the highlight box.
<img class='instructionsImage' src="https://assets.crowd.aws/images/a2i-console/multiple-lines.png" /></p>
    </short-instructions>

    <full-instructions header="Instructions"></full-instructions>
  </crowd-textract-analyze-document>
</crowd-form>
```

## Custom Template Example for Amazon Rekognition<a name="a2i-custom-templates-rekognition-sample"></a>

All custom templates begin and end with the `<crowd-form> </crowd-form>` elements\. Like standard HTML `<form>` elements, all of your form code should go between these elements\. For an Amazon Rekognition custom task template, use the `<crowd-rekognition-detect-moderation-labels>` element\. This element supports the following attributes: 
+ `categories` – An array of strings *or* an array of objects where each object has a `name` field\.
  + If the categories come in as objects, the following applies:
    + The displayed categories are the value of the `name` field\. 
    + The returned answer contains the *full* objects of any selected categories\.
  + If the categories come in as strings, the following applies:
    + The returned answer is an array of all the strings that were selected\.
+ `exclusion-category` – By setting this attribute, you create a button underneath the categories in the UI\. When a user presses the button, all categories are deselected and disabled\. If the worker presses the button again, you re\-enable users to choose categories\. If the worker submits the task by selecting the Submit button after you pressing the button, that task will return an empty array\.

For children of the `<crowd-textract-document-analysis>` element, you must have three regions\.
+ `<full-instructions>` – Instructions that are available from the **View full instructions** link in the tool\. You can leave this blank, but we recommend that you provide complete instructions to get better results\.
+ `<short-instructions>` – Brief description of the task that appears in the tool's sidebar\. You can leave this blank, but we recommend that you provide complete instructions to get better results\.

A template using these elements would look similar to the following\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
{% capture s3_arn %}http://s3.amazonaws.com/{{ task.input.aiServiceRequest.image.s3Object.bucket }}/{{ task.input.aiServiceRequest.image.s3Object.name }}{% endcapture %}

<crowd-form>
  <crowd-rekognition-detect-moderation-labels
    categories='[
      {% for label in task.input.selectedAiServiceResponse.moderationLabels %}
        {
          name: "{{ label.name }}",
          parentName: "{{ label.parentName }}",
        },
      {% endfor %}
    ]'
    src="{{ s3_arn | grant_read_access }}"
    header="Review the image and choose all applicable categories."
  >
    <short-instructions header="Instructions">
      <style>
        .instructions {
          white-space: pre-wrap;
        }
      </style>
      <p class='instructions'>Review the image and choose all applicable categories.
If no categories apply, choose None.

<b>Nudity</b>
Visuals depicting nude male or female person or persons

<b>Graphic Male Nudity</b>
Visuals depicting full frontal male nudity, often close ups

<b>Graphic Female Nudity</b>
Visuals depicting full frontal female nudity, often close ups

<b>Sexual Activity</b>
Visuals depicting various types of explicit sexual activities and pornography

<b>Illustrated Nudity or Sexual Activity</b>
Visuals depicting animated or drawn sexual activity, nudity, or pornography

<b>Adult Toys</b>
Visuals depicting adult toys, often in a marketing context

<b>Female Swimwear or Underwear</b>
Visuals depicting female person wearing only swimwear or underwear

<b>Male Swimwear Or Underwear</b>
Visuals depicting male person wearing only swimwear or underwear

<b>Partial Nudity</b>
Visuals depicting covered up nudity, for example using hands or pose

<b>Revealing Clothes</b>
Visuals depicting revealing clothes and poses, such as deep cut dresses

<b>Graphic Violence or Gore</b>
Visuals depicting prominent blood or bloody injuries

<b>Physical Violence</b>
Visuals depicting violent physical assault, such as kicking or punching

<b>Weapon Violence</b>
Visuals depicting violence using weapons like firearms or blades, such as shooting

<b>Weapons</b>
Visuals depicting weapons like firearms and blades

<b>Self Injury</b>
Visuals depicting self-inflicted cutting on the body, typically in distinctive patterns using sharp objects

<b>Emaciated Bodies</b>
Visuals depicting extremely malnourished human bodies

<b>Corpses</b>
Visuals depicting human dead bodies

<b>Hanging</b>
Visuals depicting death by hanging</p>
    </short-instructions>

    <full-instructions header="Instructions"></full-instructions>
  </crowd-rekognition-detect-moderation-labels>
</crowd-form>
```

## Add Automation with Liquid<a name="a2i-custom-templates-step2-automate"></a>

The custom template system uses [Liquid](https://shopify.github.io/liquid/) for automation\. *Liquid* is an open\-source inline markup language\. For more information and documentation, see the [Liquid homepage](https://shopify.github.io/liquid/)\.

In Liquid, the text between single curly braces and percent symbols is an instruction or *tag* that creates control flow\. Text between double curly braces is a variable or *object* that outputs its value\.

### Use Variable Filters<a name="a2i-custom-templates-step2-automate-filters"></a>

In addition to the standard Liquid filters and actions, Amazon Augmented AI \(Amazon A2I\) offers a few additional filters\. You apply filters by placing a pipe \(`|`\) character after the variable name, and then specifying a filter name\. To chain filters use the following format\.

**Example**  

```
{{ <content> | <filter> | <filter> }}
```

#### Autoescape and Explicit Escape<a name="a2i-custom-templates-step2-automate-filters-autoescape"></a>

By default, inputs are HTML\-escaped to prevent confusion between your variable text and HTML\. You can explicitly add the `escape` filter to make it more obvious to someone reading the source of your template that escaping is being done\.

#### escape\_once<a name="a2i-custom-templates-step2-automate-escapeonce"></a>

`escape_once` ensures that if you've already escaped your code, it doesn't get re\-escaped again\. For example, to ensure that `&amp;` doesn't become `&amp;amp;`\.

#### skip\_autoescape<a name="a2i-custom-templates-step2-automate-skipautoescape"></a>

`skip_autoescape` is useful when your content is meant to be used as HTML\. For example, you might have a few paragraphs of text and some images in the full instructions for a bounding box\.

****  
Use `skip_autoescape` sparingly\. As a best practice for templates, avoid passing in functional code or markup with `skip_autoescape` unless you are absolutely sure that you have strict control over what's being passed\. If you're passing user input, you could be opening your workers up to a cross\-site scripting attack\.

#### to\_json<a name="a2i-custom-templates-step2-automate-tojson"></a>

`to_json` encodes data that you provide to JavaScript Object Notation \(JSON\)\. If you provide an object, it serializes it\.

#### grant\_read\_access<a name="a2i-custom-templates-step2-automate-grantreadaccess"></a>

`grant_read_access` takes an Amazon Simple Storage Service \(Amazon S3\) URI and encodes it into an HTTPS URL with a short\-lived access token for that resource\. This makes it possible to display photo, audio, or video objects stored in S3 buckets that are not otherwise publicly accessible to workers\.

**Example of the to\_json and grant\_read\_access filters**  
Input  

```
auto-escape: {{ "Have you read 'James & the Giant Peach'?" }}
explicit escape: {{ "Have you read 'James & the Giant Peach'?" | escape }}
explicit escape_once: {{ "Have you read 'James &amp; the Giant Peach'?" | escape_once }}
skip_autoescape: {{ "Have you read 'James & the Giant Peach'?" | skip_autoescape }}
to_json: {{ jsObject | to_json }}                
grant_read_access: {{ "s3://examplebucket/myphoto.png" | grant_read_access }}
```

**Example**  
Output  

```
auto-escape: Have you read &#39;James &amp; the Giant Peach&#39;?
explicit escape: Have you read &#39;James &amp; the Giant Peach&#39;?
explicit escape_once: Have you read &#39;James &amp; the Giant Peach&#39;?
skip_autoescape: Have you read 'James & the Giant Peach'?
to_json: { "point_number": 8, "coords": [ 59, 76 ] }
grant_read_access: https://s3.amazonaws.com/examplebucket/myphoto.png?<access token and other params>
```

**Example of an automated classification template\.**  
To automate this simple text classification sample, include the Liquid tag `{{ task.input.source }}`\. This example uses the [crowd\-classifier](sms-ui-template-crowd-classifier.md) element\.  

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
      If none seems to match, choose "other."
    </full-instructions>

    <short-instructions>
      Pick the term that best describes the sentiment 
      of the tweet. 
    </short-instructions>

  </crowd-classifier>
</crowd-form>
```

## Preview a Worker Task Template<a name="a2i-preview-your-custom-template"></a>

To preview a custom worker task template, use the SageMaker `RenderUiTemplate` operation\. You can use the `RenderUiTemplate` operation with the AWS CLI or your preferred AWS SDK\. For documentation on the supported language specific SDK's for this API operation use the [ `See Also`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html#API_RenderUiTemplate_SeeAlso) section of the [ `RenderUiTemplate`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html)\. 

**Prerequisites**

To preview your worker task template, the AWS Identity and Access Management \(IAM\) role Amazon Resource Name \(ARN\), or `RoleArn`, that you use must have permission to access to the S3 objects that are used by the template\. To learn how to configure your role or user see [Enable Worker Task Template Previews ](a2i-permissions-security.md#permissions-for-worker-task-templates-augmented-ai)\.

**To preview your worker task template using the `RenderUiTemplate` operation:**

1. Provide a **`RoleArn`** of the role with required policies attached to preview your custom template\. 

1. In the **`Input`** parameter of **`Task`**, provide A JSON object that contains values for the variables defined in the template\. These are the variables that are substituted for the `task.input.source` variable\. For example, if you define a variable task\.input\.text in your template, you can supply the variable in the JSON object as "text": "sample text"\.

1. In the **`Content`** parameter of **`UiTemplate`**, insert your template\.

Once you've configured `RenderUiTemplate`, use your prefered SDK or the AWS CLI to submit a request to render your template\. If your request was successful, the response will include [ `RenderedContent`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html#API_RenderUiTemplate_ResponseSyntax), a Liquid template that renders the HTML for the worker UI\.

**Important**  
To preview your template, you need an IAM role with permissions to read Amazon S3 objects that get rendered on your user interface\. For a sample policy that you can attach to your IAM role to grant these permissions, see [Enable Worker Task Template Previews ](a2i-permissions-security.md#permissions-for-worker-task-templates-augmented-ai)\. 