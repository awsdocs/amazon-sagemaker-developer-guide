# Schedule a notebook job<a name="create-notebook-auto-run"></a>

**Note**  
The notebook scheduler is built from the Amazon EventBridge, SageMaker Training, and SageMaker Pipelines services\. If your notebook jobs fail, you might see errors related to these services\.

To schedule a notebook job, complete the following steps\.

1. Open the **Create Job** form in one of two ways:
   + Using the **File Browser**

     1. In the **File Browser** in the left panel, right\-click on the notebook you want to run as a scheduled job\.

     1. Choose **Create Notebook Job**\.
   + Within the Studio notebook
     + Inside the Studio notebook you want to run as a scheduled job, choose the **Create Notebook Job** icon in the Studio toolbar\.

1. Complete the pop\-up form\. The form displays the following fields:
   + **Job name**: A descriptive name you specify for your job\.
   + **Input file**: The name of the notebook which you are scheduling to run in non\-interactive mode\.
   + **Compute type**: The type of Amazon EC2 instance in which you want to run your notebook\.
   + **Parameters**: Custom parameters you can optionally specify as inputs to your notebook\. To use this feature, you must tag a specific cell in your Jupyter notebook with the **parameters** tag, or else your parameters may not have correct values in the notebook\. For more details, see [Parameterize your notebook](notebook-auto-run-troubleshoot-override.md)\.
   + **Additional Options**: For details about additional customizations you can apply, see [Additional options](create-notebook-auto-execution-advanced.md)\.

1. Schedule your job\. You can run your notebook on demand or on a fixed schedule\.
   + To run the notebook on demand, complete the following steps:
     + Select **Run Now**\.
     + Choose **Create**\.
     + The **Notebook Jobs** tab appears\. Choose **Reload** to load your job into the dashboard\.
   + To run the notebook on a fixed schedule, complete the following steps:
     + Choose **Run on a schedule**\.
     + Choose the **Interval** dropdown list and select an interval\. The intervals range from every minute to monthly\. You can also select **Custom schedule**\.
     + Based on the interval you choose, additional fields appear to help you further specify your desired run day and time\. For example, if you select **Day** for a daily run, an additional field appears for you to specify the desired time\. Note that any time you specify is in UTC format\. Note also that if you choose a small interval, such as one minute, your jobs overlap if the previous job is not complete when the next job starts\.

       If you select a custom schedule, you use cron syntax in the expression box to specify your exact run date and time\. The cron syntax is a space\-separated list of digits, each of which represent a unit of time from seconds to years\. For help with cron syntax, you can choose **Get help with cron syntax** under the expression box\.
     + Choose **Create**\.
     + The **Notebook Job Definitions** tab appears\. Choose **Reload** to load your job definition into the dashboard\.