# Complete a shadow test<a name="shadow-tests-complete"></a>

 Your test automatically completes at the end of the scheduled duration, or you can stop an in\-progress test early\. After your test has completed, the test’s status in the **Shadow tests** section on the **Shadow tests** page shows as **Complete**\. Then you can review and analyze the final metrics of your test\. 

 You can use the metrics dashboard to decide whether to promote the shadow variant to production\. For more information about analyzing the metrics dashboard of your test, see [Monitor a shadow test](shadow-tests-view-monitor-edit.md#shadow-tests-view-monitor-edit-dashboard)\. 

 For instructions on how to complete your test before the end of its scheduled completion time, see [Complete a shadow test early](#shadow-tests-complete-early)\. 

 For instructions on promoting your shadow variant to production, see [Promote a shadow variant](#shadow-tests-complete-promote)\. 

## Complete a shadow test early<a name="shadow-tests-complete-early"></a>

 One reason you might want to complete an in\-progress shadow test is if you’ve decided that the metrics for your shadow variant look good and you want to promote it to production\. You might also decide to complete the test if one or more of the variants aren’t performing well\. 

 To complete your test before its scheduled end date, do the following: 

1.  Select the test you want to mark complete from the **Shadow tests** section on the **Shadow tests** page\. 

1.  From the **Actions** dropdown list, choose **Complete**, and the **Complete shadow test** dialog box appears\. 

1.  In the dialog box, choose one of the following options: 
   + **Yes, deploy shadow variant**
   + **No, remove shadow variant**

1.  \(Optional\) In the **Comment** text box, enter your reason for completing the test before its scheduled end time\. 

1. 

   1.  If you decided to deploy the shadow variant, choose **Complete and proceed to deploy**\. The **Deploy shadow variant** page appears\. For instructions on how to fill out this page, see [Promote a shadow variant](#shadow-tests-complete-promote)\. 

   1.  If you decide to remove the shadow variant, choose **Confirm**\. 

## Promote a shadow variant<a name="shadow-tests-complete-promote"></a>

 If you’ve decided that you want to replace your production variant with your shadow variant, you can update your endpoint and promote your shadow variant to respond to inference requests\. This removes your current production variant from production and replaces it with your shadow variant\. 

 If your shadow test is still in\-progress, you must first complete your test\. To complete your shadow test before its scheduled end, follow the instructions in [Complete a shadow test early](#shadow-tests-complete-early) before continuing with this section\. 

 When you promote a shadow variant to production, you have the following options for the instance count of the shadow variant\. 
+  You can retain the instance count and type from the production variant\. If you select this option, then your shadow variant launches in production with the current instance count, ensuring that your model can continue to process request traffic at the same scale\. 
+  You can retain the instance count and type of your shadow variant\. If you want to use this option, we recommend that you shadow test with 100 percent traffic sampling to ensure that the shadow variant can process request traffic at the current scale\. 
+  You can use custom values for the instance count and type\. If you want to use this option, we recommend that you shadow test with 100 percent traffic sampling to ensure that the shadow variant can process request traffic at the current scale\. 

 Unless you are validating the instance type or count or both of the shadow variant, we highly recommend that you retain the instance count and type from the production variant when promoting your shadow variant\. 

 To promote your shadow variant, do the following: 

1.  If your test has completed, do the following: 

   1.  Select the test from the **Shadow test** section on the **Shadow tests** page\. 

   1.  From the **Actions** dropdown list, choose **View**\. The dashboard appears\. 

   1.  Choose **Deploy shadow variant** in the **Environment** section\. The **Deploy shadow variant** page appears\. 

    If your test has not completed, see [Complete a shadow test early](#shadow-tests-complete-early) to complete it\. 

1.  In the **Variant settings** section, select one of the following options: 
   + **Retain production settings**
   + **Retain shadow settings**
   + **Custom instance settings**

    If you selected **Custom instance settings**, do the following: 

   1.  Select the instance type from the **Instance type** dropdown list\. 

   1.  Under **Instance count**, enter the number of instances\. 

1.  In **Enter 'deploy' to confirm deployment** text box, enter **deploy**\. 

1.  Choose **Deploy shadow variant**\. 

 Your SageMaker Inference endpoint is now using the shadow variant as your production variant, and your production variant has been removed from the endpoint\. 