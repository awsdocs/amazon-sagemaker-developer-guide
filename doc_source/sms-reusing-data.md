# Chaining labeling jobs<a name="sms-reusing-data"></a>

Amazon SageMaker Ground Truth can reuse datasets from prior jobs in two ways: cloning and chaining\.

Cloning is a relatively straightforward operation\. Cloning copies the set\-up of a prior labeling job and allows you to make additional changes, before setting it to run\.

Chaining is a more complex operation\. Chaining uses not only the setup of the prior job, but the results\. This allows you to continue an incomplete job,and add labels or data objects to a completed job\. 

When it comes to the data being processed: 
+  **Cloning** — uses the prior job's *input* manifest as the new job's input manifest\. 
+  **Chaining** — uses the prior job's *output* manifest as the new job's input manifest\. 

## Chaining labeling jobs<a name="sms-reusing-data-chaining"></a>

Some situations where chaining is useful include:
+ Continue a labeling job that was manually stopped\.
+ Continue a labeling job that failed mid\-job, with issues fixed\.\.
+ Switch to automated labeling after manually labeling part of a job \(or vice\-versa\)\.
+ Add more data objects to a completed job and start the job from there\.
+ Add another annotation to a completed job\. For example, you have a collection of phrases labeled for topic, then want to run the set again, categorizing them by the topic's implied audience\.

In Amazon SageMaker Ground Truth you can configure a chained labeling job via either the console or API\.

## Key Term: Label attribute name<a name="sms-reusing-data-LAN"></a>

The label attribute name \(`LabelAttributeName` in the API\) is a string used as the key for the key\-value pair formed with the label that a worker assigns to the data object\.

There are a few rules for the label attribute name\.
+ It cannot end with `-metadata`\.
+ The names `source` and `source-ref` are reserved and cannot be used\.
+ Semantic\-segmentation labeling jobs require it to end with `-ref`\. All other labeling jobs require it to *not* end with `-ref`\. The adding of `-ref` is managed automatically for you in jobs configured via the console\.
+ If you're using the same label attribute name from the originating job and you configure the chained job to use auto\-labeling, then if it had been in auto\-labeling mode at any point, the model from the originating job is used\.

In an output manifest, it can appear something like this:

```
  "source-ref": "<S3 URI>",
  "<label attribute name>": {
    "annotations": [{
      "class_id": 0,
      "width": 99,
      "top": 87,
      "height": 62,
      "left": 175
    }],
    "image_size": [{
      "width": 344,
      "depth": 3,
      "height": 234
    }]
  },
  "<label attribute name>-metadata": {
    "job-name": "<job name>",
    "class-map": {
      "0": "<label attribute name>"
    },
    "human-annotated": "yes",
    "objects": [{
      "confidence": 0.09
    }],
    "creation-date": "<timestamp>",
    "type": "groundtruth/object-detection"
  }
```

If you're creating a job in the console, the job name is used as the label attribute name for the job if you don't explicitly set the label attribute name value\.

## Starting a chained job in the console<a name="sms-reusing-data-console"></a>

Select a stopped, failed, or completed labeling job from the list of your existing jobs\. This enables the **Actions** menu\.

From the **Actions** menu, select **Chain**\.

### Job overview panel<a name="sms-reusing-data-console-job-panel"></a>

In the **Job overview** panel, a new **Job name** is set based on the title of the job from which you are chaining this one\. You can change it\.

You may also specify a label attribute name different from the labeling job name\.

If you're chaining from a completed job, the label attribute name uses the name of the new job you're configuring\. To change the name, select the check box\.

If you're chaining from a stopped or failed job, the label attribute name uses to the name of the job from which you're chaining\. Its easy to see and edit the value because the name check box is checked\.

**Attribute label naming considerations**  
**The default** uses the label attribute name Ground Truth has selected\. All data objects without data connected to that label attribute name are labeled\.
**Using a label attribute name** not present in the manifest causes the job to process *all* the objects in the dataset\.

The **input dataset location** in this case is automatically selected as the output manifest of the chained job\. The input field is not available, so you cannot change it\.

**Adding data objects to a labeling job**  
You cannot specify an alternate manifest file\. Manually edit the output manifest from the previous job to add new items before starting a chained job\. The S3 URI helps you locate where you are storing the manifest in your S3 bucket\. Download the manifest file from there, edit it locally on your computer, then upload the new version to replace it\. Make sure you are not introducing errors during editing\. We recommend you use JSON linter to check your JSON\. Many popular text editors and IDEs have linter plugins available\.

## Starting a chained job with the API<a name="sms-reusing-data-API"></a>

The procedure is almost the same as setting up a new labeling job with `CreateLabelingJob`, except for two primary differences\.
+ **Manifest location:** Rather than use your original manifest from the prior job, the value for the `ManifestS3Uri` in the `DataSource` should point to the S3 URI of the *output manifest* from the prior labeling job\.
+ **Label attribute name:** Setting the correct `LabelAttributeName` value is important here\. As pointed out, this is the key portion of a key\-value pair where labeling data is the value\. Sample use cases include:
  + **Adding new or more\-specific labels to a completed job** — set a new label attribute name\.
  + **Labeling the unlabeled items from a prior job** — use the label attribute name from the prior job\.

## Using a partially labeled dataset<a name="sms-reusing-data-newdata"></a>

You can get some chaining benefits if you use an augmented manifest that has already been partially labeled\. Check the **Label attribute name** check box and set the name so that it matches the name in your manifest\.

If you're using the API, the instructions are the same as starting a chained job\. However, be sure to upload your manifest to an S3 bucket and use it instead of using the output manifest from a prior job\.

The **Label attribute name** value in the manifest has to conform to the naming considerations discussed above\.