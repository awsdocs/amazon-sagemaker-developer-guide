# Creating Good Worker Instructions<a name="a2i-creating-good-instructions-guide"></a>

Creating good instructions for your human review jobs improves your worker's accuracy in completing their task\. You can modify the default instructions that are provided in the console when creating a human review workflow, or you can use the console to create a custom worker template and include your instructions in this template\. The instructions are shown to the worker on the UI page where they complete their labeling task\.

## Create Good Worker Instructions<a name="a2i-good-instructions-console"></a>

There are three kinds of instructions in the Amazon Augmented AI console:
+ **Task Description** – The description should provide a succinct explanation of the task\.
+ **Instructions** – These instructions are shown on the same webpage where workers complete a task\. These instructions should provide an easy reference to show the worker the correct way to complete the task\.
+ **Additional Instructions** – These instructions are shown in a dialog box that appears when a worker chooses **View full instructions**\. We recommend that you provide detailed instructions for completing the task, and include several examples showing edge cases and other difficult situations for labeling objects\.

## Add Example Images to Your Instructions<a name="sms-using-s3-images"></a>

Images provide useful examples for your workers\. To add a publicly accessible image to your instructions, do the following:

1. Place the cursor where the image should go in the instructions editor\.

1. Choose the image icon in the editor toolbar\.

1. Enter the URL of your image\.

If your instruction image is in an S3 bucket that isn't publicly accessible, do the following:
+ For the image URL, enter: `{{ 'https://s3.amazonaws.com/your-bucket-name/image-file-name' | grant_read_access }}`\.

This renders the image URL with a short\-lived, one\-time access code that's appended so the worker's browser can display it\. A broken image icon is displayed in the instructions editor, but previewing the tool displays the image in the rendered preview\. See [grant\_read\_access](a2i-custom-templates.md#a2i-custom-templates-step2-automate-grantreadaccess) for more information about the `grand_read_access` element\. 