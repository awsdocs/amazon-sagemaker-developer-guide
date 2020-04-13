# crowd\-rekognition\-detect\-moderation\-labels<a name="a2i-crowd-rekognition-detection"></a>

A widget to enable human review of an Amazon Rekognition image moderation result\.

### Attributes<a name="rekognition-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="rekognition-attributes-header"></a>

This is the text that is displayed as the header\.

#### src<a name="rekognition-attributes-src"></a>

This is a link to the image to be analyzed by the worker\. 

#### categories<a name="rekognition-attributes-categories"></a>

This supports `categories` as an array of strings **or** an array of objects where each object has a `name` field\.

If the categories come in as objects, the following applies:
+ The displayed categories are the value of the `name` field\.
+ The returned answer contains the **full** objects of any selected categories\.

If the categories come in as strings, the following applies:
+ The returned answer is an array of all the strings that were selected\.

#### exclusion\-category<a name="rekognition-attributes-exclusion-category"></a>

By setting this attribute you create a button underneath the categories in the UI\. 
+ When a user chooses the button, all categories are deselected and disabled\.
+ Choosing the button again re\-enables the categories so that users can choose them\.
+ If you submit after choosing the button, it returns an empty array\.

### Element Hierarchy<a name="rekognition-crowd-element-hierarchy"></a>

This element has the following parent and child elements\.
+ Parent elements – crowd\-form
+ Child elements – [full\-instructions](#rek-full-instructions), [short\-instructions](#rek-short-instructions) 

### AWS Regions<a name="rek-crowd-regions"></a>

The following AWS Regions are supported by this element\. You can use custom HTML and CSS code within these Regions to format your instructions to workers\. For example, use the `short-instructions` section to provide good and bad examples of how to complete a task\. 

#### full\-instructions<a name="rek-full-instructions"></a>

General instructions about how to work with the widget\. 

#### short\-instructions<a name="rek-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\. 

### Example Worker Template with the crowd Element<a name="rek-crowd-element-example"></a>

An example of a worker template using the crowd element would look like the following\.

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
Visuals depicting animated or drawn sexual activity, nudity or pornography

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

### Output<a name="rek-crowd-element-output"></a>

The following is a sample of the output from this element\. For details about this output, see Amazon Rekognition [DetectModerationLabels](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectModerationLabels.html) API documentation\. 

```
{
  "AWS/Rekognition/DetectModerationLabels/Image/V3": {
    "ModerationLabels": [
        { name: 'Gore', parentName: 'Violence' },
        { name: 'Corpses', parentName: 'Violence' },
    ]
  }
}
```