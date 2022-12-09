# Getting started with using Amazon SageMaker Canvas<a name="canvas-getting-started"></a>

This guide tells you how to get started with using SageMaker Canvas\. If you're an IT administrator, see [Setting Up and Managing Amazon SageMaker Canvas \(for IT Administrators\)](canvas-setting-up.md) to set up SageMaker Canvas for your users\.

If you're a business user or analyst, read the following sections\.

**Topics**
+ [Prerequisites for setting up Amazon SageMaker Canvas](#canvas-prerequisites)
+ [Step 1: Log in to Amazon SageMaker Canvas as a business user](#canvas-getting-started-step1)
+ [Step 2: Import and manage data](#canvas-getting-started-step2)
+ [Step 3: Build a model](#getting-started-step3)
+ [Step 4: Evaluate your model](#canvas-getting-started-step4)
+ [Step 5: Make predictions](#canvas-getting-started-step5)

## Prerequisites for setting up Amazon SageMaker Canvas<a name="canvas-prerequisites"></a>

To set up Amazon SageMaker Canvas, you either contact your administrator or do the following:
+ Set up an Amazon SageMaker Domain
+ Optional: Give yourself the ability to upload local files
+ Optional: Give yourself permissions to do time series forecasts
+ Optional: Give yourself permissions to import Amazon Redshift data

**Important**  
For you to set up Amazon SageMaker Canvas, your version of Amazon SageMaker Studio must be 3\.19\.0 or later\. For information about updating Amazon SageMaker Studio, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.

**To onboard to Domain using IAM Identity Center**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Control Panel** at the top left of the page\.

1. On the **Control Panel** page, under **Choose setup method**, choose **Standard setup**\.

1. Select **Configure**\.

Use the following procedure to configure the general settings\.

1. Under **Permission**, for **IAM role**, choose an option from the role selector\.

   If you choose **Enter a custom IAM role ARN**, the role must have at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:

   1. Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. Under **Network and storage**, specify the following:
   + Your VPC information – For more information, see [Choose a VPC](onboard-vpc.md) and [Configure Amazon SageMaker Canvas in a VPC without internet access](canvas-vpc.md)\.
   + \(Optional\) **Encryption key** – SageMaker uses an AWS KMS key to encrypt your Amazon Elastic File System \(Amazon EFS\) and Amazon Elastic Block Store \(Amazon EBS\) file systems\. By default, it uses an AWS managed key\. To use a customer managed key, enter its key ID or Amazon Resource Name \(ARN\)\. For more information, see [Protect Data at Rest Using Encryption](encryption-at-rest.md)\.
**Note**  
Encryption in transit is only available for Amazon SageMaker Studio\.

1. Select **Next**\. 

Use the following procedure to configure the SageMaker Canvas settings\.

1. For the **Canvas base permissions configuration**, leave the **Enable Canvas base permissions** option turned on \(it is turned on by default\)\. This establishes the minimum required permissions to use the SageMaker Canvas app\.

1. \(Optional\) For the **Time series forecasting configuration**, leave the **Enable time series forecasting** option turned on to give your users permissions to do time series forecasting in SageMaker Canvas \(it is turned on by default\)\.

1. \(Optional\) If you left **Enable time series forecasting** turned on, select **Create and use a new execution role**, or select **Use an existing execution role** if you already have an IAM role with the required Amazon Forecast permissions attached \(for more information, see the [IAM role setup method](canvas-set-up-forecast.md#canvas-set-up-forecast-iam)\)\.

1. \(Optional\) Add **Tags** to track your cost and usage trends in AWS Billing and Cost Management\. SageMaker adds the tags you specify in the Domain to all of the SageMaker Canvas apps you create in the Domain\. For more information about billing and tags, see [Manage billing and cost in SageMaker Canvas](canvas-manage-cost.md)\.

1. Finish making any other changes to your Domain setup, and then choose **Submit**\.

If you encounter an error during post\-building analysis that tells you to increase your quota for `ml.m5.2xlarge` instances, use the following information to resolve the issue\. To allow SageMaker Canvas to complete post\-building analysis of models, you must increase the SageMaker Hosting endpoint limit for the `ml.m5.2xlarge` instance type to a non\-zero value in your AWS account\. After building a model, SageMaker Canvas hosts the model on a SageMaker Hosting endpoint and uses the endpoint to generate the post\-building analysis\. If you don't increase the default account limit of 0 for `ml.m5.2xlarge` instances, SageMaker Canvas cannot complete this step and generates an error during post\-building analysis\.

Use the following procedure to request a limit increase for your account\.

1. Open the [AWS Support Center console](https://console.aws.amazon.com/support/home#/case/create)\.

1. On the **AWS Support Center** page, choose **Create Case** and then choose **Service limit increase**\.

1. In the **Case classification** panel under **Limit type**, search for SageMaker\.

1. In the **Request** panel, choose the **Region** that you are working in\. For **Resource Type**, choose **SageMaker Hosting**\.

1. For **Limit**, choose **ml\.m5\.2xlarge** instances\.

1. For **New Limit Value**, verify that the value is at least **1**\.

1. In **Case description**, provide a brief explanation of why you need the **Service limit increase**\. For example, "SageMaker Canvas uses this instance type for model analysis\."

1. In **Contact options**, provide some details about how you would like to be contacted by the AWS service support team on the status of your **Service limit increase** request\.

1. Choose **Submit**\.

You can now access SageMaker Canvas by doing the following\.

1. Navigate to the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Under **Control Panel**, choose **Canvas**\.

1. Choose **Launch app**\.

1. Choose **Canvas**\.

SageMaker Canvas creates an Amazon S3 bucket with a name that uses the following pattern: `sagemaker-Region-your-account-id`\.

If you'd like to have the ability to upload files from your local machine to SageMaker Canvas, you attach a CORS policy to it\.

To attach a CORS policy, use the following procedure\.

1. Sign in to [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket with the name that uses the following pattern: `sagemaker-AWS-Region-AWS-account-id`\.

1. Choose **Permissions**\.

1. Navigate to **Cross\-origins resource sharing \(CORS\)**\.

1. Choose **Edit**\.

1. Add the following CORS policy:

   ```
   [
       {
           "AllowedHeaders": [
               "*"
           ],
           "AllowedMethods": [
               "POST"
           ],
           "AllowedOrigins": [
               "*"
           ],
           "ExposeHeaders": []
       }
   ]
   ```

1. Choose **Save changes**\.

After updating the CORS policy, you still might not be successful in uploading your files\. The browser might be caching the CORS settings from a previous upload attempt\. If you're running into issues, clear your browser cache and try again\.

You might want to give yourself the ability to perform forecasts on time series data\. You can add time series forecasting permissions when setting up your Domain, or you can edit the permissions for a user profile after creating your Domain\. The required permissions are the `AmazonSageMakerCanvasForecastAccess` managed policy and a trust relationship with Amazon Forecast to the AWS IAM role you chose when setting up the user profile\. For instructions on how to add these permissions to your IAM role, see [Grant Your Users Permissions to Perform Time Series Forecasting](canvas-set-up-forecast.md)\.

If you want to import data from Amazon Redshift, you must give yourself additional permissions\. You must add the `AmazonRedshiftFullAccess` managed policy to the AWS IAM role you chose when setting up the user profile\. For instructions on how to add the policy to the role, see [Grant Users Permissions to Import Amazon Redshift Data](canvas-redshift-permissions.md)\.

## Step 1: Log in to Amazon SageMaker Canvas as a business user<a name="canvas-getting-started-step1"></a>

Contact your administrator to guide you through the process of setting up Amazon SageMaker Canvas\. When you log into SageMaker Canvas for the first time, there is a welcome message with quick getting started tutorials that you can follow for a walkthrough of the SageMaker Canvas application\.

![\[Screenshot of the welcome message with the getting started tutorials in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-sample-datasets-getting-started.png)

You can follow the **Get started with Canvas** tutorial for a high\-level overview of the SageMaker Canvas application\. There are also shorter tutorials that guide you through the individual steps of using SageMaker Canvas\. These tutorials show you how to import a dataset, build a model, analyze the results of a built model, and generate predictions with your model\. You can revisit the tutorials at any time by choosing the **Help** button and then choosing **Quick tutorials** on the left navigation bar inside the SageMaker Canvas application\.

![\[Screenshot of the help panel in the SageMaker Canvas application where you can access the getting started tutorials.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-sample-datasets-help-pane.png)

## Step 2: Import and manage data<a name="canvas-getting-started-step2"></a>

Format your data so that you can analyze it in Amazon SageMaker Canvas by importing it into a dataset\. You import data from multiple sources into a single dataset\.

You can import data from the following sources:
+ Local files
+ Amazon S3
+ Amazon Redshift
+ Snowflake

Use the following procedure to import a dataset\.

To import your data, do the following\.

1. In the left navigation pane, choose **Datasets**\.

1. Choose **Import Data**\.

1. Optional: Add a connection to an external data source, such as an external Amazon S3 bucket, Amazon Redshift, or Snowflake\. For more information about importing data, see [Use Snowflake with Amazon SageMaker Canvas](canvas-connecting-external.md#canvas-using-snowflake)\.

1. Select one or more files from your Amazon S3 bucket or your local folder\. Your data must meet the following requirements:
   + Your file can't exceed 5 GB\.
   + Currently, your file must be in \.csv format\. Its values must be comma delimited and must not have newline characters except when denoting a new row\.
   + Your data can't have more than 1000 columns\.

1. Optional: To preview the datasets that you've uploaded and to review the headers, navigate to the section following the datasets that you're uploading\.

1. Choose **Import**\.

The following images show how SageMaker Canvas previews files that you've uploaded locally by choosing **Preview**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-preview-0.png)

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-preview-1.png)

The following image shows the import page for datasets that are stored on Amazon S3\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-s3-upload.png)

The preceding information walks you through how to import the data\. For more information, see [ Importing data in Amazon SageMaker Canvas](canvas-importing-data.md)\.

## Step 3: Build a model<a name="getting-started-step3"></a>

Build a model that you can use to make predictions on new data\. To build a model, you choose the **Target column** in your dataset for which you want to make predictions\. Amazon SageMaker Canvas looks at the data in the column and makes recommendations for the types of models that you can train\. Choose the model type that works best for your use case\.

You can choose **Preview model** before you build the model to get a sense of how well the model can make predictions\. The prediction accuracy for **Preview model** is generally lower than the actual prediction accuracy of the model you've built\. However, it is usually similar to the value of the model\.

The following information shows you how to build a model and provides you with contextual information\. For more information about model building, see [Build a model](canvas-build-model.md)\.

Use the following procedure to build a model\.

1. In the navigation pane, choose **Models**\.

1. Choose **New Model** and specify a name for the model\.

1. Choose the data that you want to use to build a model\. If you haven't imported the data yet, you can choose **Import data**\.

1. Select the target that you would like to predict out of the columns in the dropdown list\. SageMaker Canvas automatically chooses the problem type for you\.

1. Optional: Choose the checkboxes next to the names of the columns to drop them from the dataset\.

1. Optional: Choose **Analyze data** to get a general sense of the model's performance before you build it\.

1. Choose the downward arrow next to **Quick build**\.

1. Choose **Quick build** or **Standard build**\.

The following image shows the **Quick build** and **Standard build** options\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-build/canvas-build-quick-or-standard.png)

The following image shows the data in a table view\. Choosing a column opens descriptive statistics and visualizations for the column\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-build/canvas-build-model-column-info.png)

The following image shows the columns in a dataset\. The checkboxes that have been grayed out indicate that they won't be used to build the model\. You can see that the unselected boxes appear in the **Model Recipe**\. The model recipe lists the changes that you've made to the dataset that you've provided\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-build/canvas-build-model-recipe.png)

The following image shows how choosing **Preview model** quickly creates an analysis of how well a fully built model may perform and which columns had the most impact on the models predictions\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-build/canvas-build-preview-model.png)

After you start model building, Amazon SageMaker Canvas automatically cleans and pre\-processes your data\. It builds up to 250 models and chooses the one that is the most accurate\. The time it takes for Amazon SageMaker Canvas to build a model depends on whether you're doing a **Quick build** or a **Standard build** and the size of the dataset\. A **Quick build** usually takes 2\-20 minutes to build, whereas a **Standard build** usually takes 2\-4 hours to build\. You can safely navigate away from the model building page and come back to it when SageMaker Canvas finishes building your model\.

The following image shows the process of model building\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-build/canvas-build-building.png)

## Step 4: Evaluate your model<a name="canvas-getting-started-step4"></a>

Before using your model to make predictions on new data, you can evaluate how well it performed\. You can use information such as the impact that each column had on the predictions\. The following image shows an example evaluation page with an explanation of the score and the column impact\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-column-impact-0.png)

You can use the **Scoring** tab to get visualizations and metrics on your model's ability to make predictions\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-analyze-regression-scoring.png)

## Step 5: Make predictions<a name="canvas-getting-started-step5"></a>

You can make one of the following types of predictions\.
+ Batch – Predictions for an entire dataset\.
+ Single – Predictions for a single value that you specify\.

For each set of predictions, SageMaker Canvas returns the following:
+ The predicted values
+ The probability of the predicted value being correct
+ The dataset that you've specified for generating predictions

Use the following procedure to make a single prediction with your model\.

1. Choose **Single prediction**\.

1. Change the input values to see how the predicted value changes from the average prediction\.

1. Choose **Update** to get the new prediction\.

![\[Screenshot of a single prediction in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/beta-2-pred-3.png)

To make batch predictions, choose a dataset and use the following procedure to generate batch predictions\.

1. Choose **Batch prediction**\.

1. Choose **Select Dataset**\.

1. Choose the dataset\.

1. Choose **Generate predictions**\.

The following images visualize the preceding procedure\.

![\[Screenshot of selecting batch prediction in the SageMaker Canvas application\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/beta-2-pred-1.png)

![\[Screenshot of selecting a dataset for batch prediction in the SageMaker Canvas application\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/beta-2-pred-2.png)

For batch predictions, you can download the model's predictions as a \.csv file\. When the **Status** of the predictions output is **Ready**, you can download the file by choosing the **More options** icon \(![\[More options icon for the output CSV file.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/more-options-icon.png)\) and then choosing **Download**\. If you dropped any columns when building your model, SageMaker Canvas adds the dropped columns back to the batch predictions\.

**Note**  
SageMaker Canvas does not add the dropped columns to your batch predictions for time series models\.

You might not be able to make predictions on some datasets because they might have incompatible schemas\. A schema is the organizational structure\. For a dataset, it is the names of the columns and the data type of the data in the columns\.

An incompatible schema might happen for one of the following reasons:
+ The dataset that you're using to make predictions has fewer columns than the dataset that you're using to build the model\.
+ The data types in the columns you used to build the dataset might be different from the data types in dataset that you're using to make predictions\.
+ The dataset that you're using to make predictions and the dataset that you've used to build the model have column names that don't match\. The column names are case sensitive\. "Column1" is not the same as "column1"\.