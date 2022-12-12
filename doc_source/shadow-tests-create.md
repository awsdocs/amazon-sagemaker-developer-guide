# Create a shadow test<a name="shadow-tests-create"></a>

 You can create a shadow test to compare the performance of a shadow variant against a production variant\. You can run the test on an existing endpoint that is serving inference requests or you can create a new endpoint on which to run the test\. 

 To create a shadow test you need to specify the following: 
+  A *production variant* that receives and responds to 100 percent of the incoming inference requests\. 
+  A *shadow variant* that receives a percentage of the incoming requests, replicated from the production variant, but does not return any responses\. 

 For each variant, you can use SageMaker to control the model, instance type, and instance count\. You can configure the percentage of incoming requests, known as the traffic sampling percentage, that you want replicated to your shadow variant\. SageMaker manages the replication of requests to your shadow variant and you can modify the traffic sampling percentage when your test is scheduled or running\. You can also optionally turn on Data Capture to log requests and responses of your production and shadow variants\. 

**Note**  
 SageMaker supports a maximum of one shadow variant per endpoint\. For an endpoint with a shadow variant, there can be a maximum of one production variant\. 

 You can schedule the test to start at any time and continue for a specified duration\. The default duration is 7 days and the maximum is 30 days\. After the test is complete, the endpoint reverts to the state it was in prior to starting the test\. This ensures that you do not have to manually clean up resources upon the completion of the test\. 

 You can monitor a test that is running through a dashboard in the SageMaker console\. The dashboard provides a side by side comparison of invocation metrics and instance metrics between the production and shadow variants, along with a tabular view with relevant metric statistics\. This dashboard is also available for completed tests\. Once you have reviewed the metrics, you can either choose to promote the shadow variant to be the new production variant or retain the existing production variant\. Once you promote the shadow variant, it responds to all incoming requests\. For more information, see [Promote a shadow variant](shadow-tests-complete.md#shadow-tests-complete-promote)\. 

 The following procedure describes how to create a shadow test through the SageMaker console\. There are variations in the workflow depending on whether you want to use an existing endpoint or to create a new endpoint for the shadow test\. 

**Topics**
+ [Prerequisites](#shadow-tests-create-prerequisites)
+ [Enter shadow test details](#shadow-tests-create-console-shadow-test-details)
+ [Enter shadow test settings](#shadow-tests-create-console-shadow-test-settings)

## Prerequisites<a name="shadow-tests-create-prerequisites"></a>

 Before creating a shadow test with the SageMaker console, you must have a SageMaker model ready to use\. For more information about how to create a SageMaker model, see [Create a Model](realtime-endpoints-deployment.md#realtime-endpoints-deployment-create-model)\. 

 You can get started with shadow tests with an existing endpoint with a production variant and a shadow variant, an existing endpoint with only a production variant, or just the SageMaker models you'd like to compare\. Shadow tests support creating an endpoint and adding variants before your test begins\. 

## Enter shadow test details<a name="shadow-tests-create-console-shadow-test-details"></a>

 To start creating your shadow test, fill out the **Enter shadow test details** page by doing the following: 

1.  Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\. 

1.  In the left navigation panel, choose **Inference**, and then choose **Shadow tests**\. 

1.  Choose **Create shadow test**\. 

1.  Under **Name**, enter a name for the test\. 

1.  \(Optional\) Under **Description**, enter a description for the test\. 

1.  \(Optional\) Specify **Tags** using **Key** and **Value** pairs\. 

1.  Choose **Next**\. 

## Enter shadow test settings<a name="shadow-tests-create-console-shadow-test-settings"></a>

 After filling out the **Enter shadow test details** page, fill out the **Enter shadow test settings** page\. If you already have a SageMaker Inference endpoint and a production variant, follow the **Use an existing endpoint** workflow\. If you don't already have an endpoint, follow the **Create a new endpoint** workflow\. 

------
#### [ Use an existing endpoint ]

 If you want to use an existing endpoint for your test, fill out the **Enter shadow test settings** page by doing the following: 

1.  Choose a role that has the `AmazonSageMakerFullAccess` IAM policy attached\. 

1.  Choose **Use an existing endpoint**, and then choose one of the available endpoints\. 

1.  \(Optional\) To encrypt the storage volume on your endpoint, either choose an existing KMS key or choose **Enter a KMS key ARN** from the dropdown list under **Encryption key**\. If you choose the second option, a field to enter the KMS key ARN appears\. Enter the KMS key ARN in that field\. 

1.  If you have multiple production variants behind that endpoint, remove the ones you don't want to use for the test\. You can remove a model variant by selecting it and then choosing **Remove**\. 

1.  If you do not already have a shadow variant, add a shadow variant\. To add a shadow variant, do the following: 

   1.  Choose **Add**\. 

   1.  Choose **Shadow variant**\. 

   1.  In the **Add model** dialog box, choose the model you want to use for your shadow variant\. 

   1.  Choose **Save**\. 

1.  \(Optional\) In the preceding step, the shadow variant is added with the default settings\. To modify these settings, select the shadow variant and choose **Edit**\. The **Edit shadow variant** dialog box appears\. For more information on filling out this dialog box, see [Edit a shadow test](shadow-tests-view-monitor-edit.md#shadow-tests-view-monitor-edit-individual)\. 

1.  In the **Schedule** section, enter the duration of the test by doing the following: 

   1.  Choose the box under **Duration**\. A popup calender appears\. 

   1.  Select the start and end dates from the calender, or enter the start and end dates in the fields for **Start date** and **End date**, respectively\. 

   1.  \(Optional\) For the fields **Start time** and **End time**, enter the start and end times, respectively, in the 24 hour format\. 

   1.  Choose **Apply**\. 

    The minimum duration is 1 hour, and the maximum duration is 30 days\. 

1.  \(Optional\) Turn on **Enable data capture** to save inference request and response information from your endpoint to an Amazon S3 bucket, and then enter the location of the Amazon S3 bucket\. 

1.  Choose **Create shadow test**\. 

------
#### [ Create a new endpoint ]

 If you don't have an existing endpoint, or you want to create a new endpoint for your test, fill out the **Enter shadow test settings** page by doing the following: 

1.  Choose a role that has the `AmazonSageMakerFullAccess` IAM policy attached\. 

1.  Choose **Create a new endpoint**\. 

1.  Under **Name**, enter a name for the endpoint\. 

1.  Add one production variant and one shadow variant to the endpoint: 
   +  To add a production variant choose **Add**, and then choose **Production variant**\. In the **Add model** dialog box, choose the model you want to use for your production variant, and then choose **Save**\. 
   +  To add a shadow variant choose **Add**, and then choose **Shadow variant**\. In the **Add model** dialog box, choose the model you want to use for your shadow variant, and then choose **Save**\. 

1.  \(Optional\) In the preceding step, the shadow variant is added with the default settings\. To modify these settings, select the shadow variant and choose **Edit**\. The **Edit shadow variant** dialog box appears\. For more information on filling out this dialog box, see [Edit a shadow test](shadow-tests-view-monitor-edit.md#shadow-tests-view-monitor-edit-individual)\. 

1.  In the **Schedule** section, enter the duration of the test by doing the following: 

   1.  Choose the box under **Duration**\. A popup calender appears\. 

   1.  Select the start and end dates from the calender, or enter the start and end dates under **Start date** and **End date**, respectively\. 

   1.  \(Optional\) Under **Start time** and **End time**, enter the start and end times, respectively, in the 24 hour format\. 

   1.  Choose **Apply**\. 

    The minimum duration is 1 hour, and the maximum duration is 30 days\. 

1.  \(Optional\) Turn on **Enable data capture** to save inference request and response information from your endpoint to an Amazon S3 bucket, and then enter the location of the Amazon S3 bucket\. 

1.  Choose **Create shadow test**\. 

------

 After completing the preceding procedures, you should now have a test scheduled to begin at your specified start date and time\. You can view the progress of the test from a dashboard\. For more information about viewing your test and the actions you can take, see [View, monitor, and edit shadow tests](shadow-tests-view-monitor-edit.md)\. 