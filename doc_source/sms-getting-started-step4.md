# Step 4: Configure the Bounding Box Tool<a name="sms-getting-started-step4"></a>

Finally you configure the bounding box tool to give instructions to your workers\. You can configure a task title that describes the task and provides high\-level instructions for the workers\. You can provide both quick instructions and full instructions\. Quick instructions are displayed next to the image to be labeled\. Full instructions contain detailed instructions for completing the task\. In this example, you only provide quick instructions\. You can see an example of full instructions by choosing **Full instructions** at the bottom of the section\.

**To configure the bounding box tool**

1. In the **Task description** field type in brief instructions for the task\. For example:

   **Draw a box around any *objects* in the image\.**

   Replace *objects* with the name of an object that appears in your images\.

1. In the **Labels** field, type a category name for the objects that the worker should draw a bounding box around\. For example, if you are asking the worker to draw boxes around football players, you could use "FootballPlayer" in this field\.

1. The **Short instructions** section enables you to create instructions that are displayed on the page with the image that your workers are labeling\. We suggest that you include an example of a correctly drawn bounding box and an example of an incorrectly drawn box\. To create your own instructions, use these steps:

   1. Select the text between **GOOD EXAMPLE** and the image placeholder\. Replace it with the following text:

      **Draw the box around the object with a small border\.**

   1. Select the first image placeholder and delete it\.

   1. Choose the image button and then enter the HTTPS URL of one of the images that you created in step 1\.

   1. Select the text between **BAD EXAMPLE** and the image placeholder\. Replace it with the following text:

      **Don't make the bounding box too large or cut into the object\.**

   1. Select the second image placeholder and delete it\.

   1. Choose the image button and then enter the HTTPS URL of the other image that you created in step 1\.

Configuration of your labeling job is complete\. To start your job, choose **Submit**\.

## Next<a name="step4-next"></a>

[Step 5: Monitoring Your Labeling Job](sms-getting-started-step5.md)