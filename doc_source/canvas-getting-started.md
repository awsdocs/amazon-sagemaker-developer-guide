# Getting started with using Amazon SageMaker Canvas<a name="canvas-getting-started"></a>

This guide tells you how to get started with using SageMaker Canvas\. If you're an IT administrator, see [Setting up and managing Amazon SageMaker Canvas \(for IT administrators\)](canvas-setting-up.md) to set up SageMaker Canvas for your users\.

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
+ Optional: Give yourself the ability to do time series forecasts

**Important**  
For you to set up Amazon SageMaker Canvas, your version of Amazon SageMaker Studio must be 3\.19\.0 or later\. For information about updating Amazon SageMaker Studio, see [Update SageMaker Studio](studio-tasks-update-studio.md)\.

**To onboard to Domain using AWS SSO**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **SageMaker Domain** at the top left of the page\.

1. On the **SageMaker Domain** page, under **Choose setup method**, choose **Standard setup**\.

1. Select **Configure**\.

Use the following procedure to configure the general settings\.

1. Under **Permission**, for **IAM role**, choose an option from the role selector\.

   If you choose **Enter a custom IAM role ARN**, the role must have at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:

   1. Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. Under **Network and storage**, specify the following:
   + Your VPC information – For more information, see [Choose a VPC](onboard-vpc.md) and [Run Amazon SageMaker Canvas in a VPC](canvas-vpc.md)\.
   + \(Optional\) **Encryption key** – SageMaker uses an AWS KMS key to encrypt your Amazon Elastic File System \(Amazon EFS\) and Amazon Elastic Block Store \(Amazon EBS\) file systems\. By default, it uses an AWS managed key\. To use a customer managed key, enter its key ID or Amazon Resource Name \(ARN\)\. For more information, see [Protect Data at Rest Using Encryption](encryption-at-rest.md)\.
**Note**  
Encryption in transit is only available for Amazon SageMaker Studio\.

1. Select **Next**\. 

Use the following procedure to configure Amazon SageMaker Studio\.

1. Under **Notebook Sharing Configuration**, turn on notebook sharing\.

1. Select **Next**\. 

You can now access SageMaker Canvas by doing the following\.

1. Navigate to the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Under **SageMaker Domain**, choose **Canvas**\.

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

You might want to give yourself the ability to perform forecasts on time series data\.

To give yourself the ability to do time series forecasting, add `AmazonForecastFullAccess` to your IAM role\.

1. Add the `AmazonForecastFullAccess` AWS managed policy to your IAM role\.

1. Add the following trust relationship to the IAM role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
            "sagemaker.amazonaws.com", # Already present
            "forecast.amazonaws.com"   # This needs to be added
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

For information about AWS managed policies, see [Managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html)\.

## Step 1: Log in to Amazon SageMaker Canvas as a business user<a name="canvas-getting-started-step1"></a>

Contact your administrator to guide you through the process of setting up Amazon SageMaker Canvas\. When you gain access, you see the following page\. You can choose **Getting Started** for a walkthrough of the service\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-get-started.png)

The following image shows the beginning of the **Getting Started** tutorial\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-get-started-page.png)

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

After you start model building, Amazon SageMaker Canvas automatically cleans and pre\-processes your data\. It builds up to 250 models and chooses the one that is the most accurate\. The time it takes for Amazon SageMaker Canvas to build a model depends on whether you're doing a **Quick build** or a **Standard build** and the size of the dataset\. A **Quick build** usually takes 2\-15 minutes to build, whereas a **Standard build** usually takes 2\-4 hours to build\. You can safely navigate away from the model building page and come back to it when SageMaker Canvas finishes building your model\.

The following image shows the process of model building\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-build/canvas-build-building.png)

## Step 4: Evaluate your model<a name="canvas-getting-started-step4"></a>

Before using your model to make predictions on new data, you can evaluate how well it performed\. You can use information such as the impact that each column had on the predictions\. The following image shows an example evaluation page with an explanation of the score and the column impact\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-column-impact-0.png)

You can use the **Scoring** tab to get visualizations and metrics on your model's ability to make predictions\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-analyze-regression-scoring.png)

## Step 5: Make predictions<a name="canvas-getting-started-step5"></a>

You might not be able to make predictions on some datasets because they might have incompatible schemas\. A schema is the organizational structure\. For a dataset, it is the names of the columns and the data type of the data in the columns\.

An incompatible schema might happen for one of the following reasons:
+ The dataset that you're using to make predictions has fewer columns than the dataset that you're using to build the model\.
+ The data types in the columns you used to build the dataset might be different from the data types in dataset that you're using to make predictions\.
+ The dataset that you're using to make predictions and the dataset that you've used to build the model have column names that don't match\. The column names are case sensitive\. "Column1" is not the same as "column1"\.

You can make one of the following types of predictions\.
+ Batch – Predictions for an entire dataset\.
+ Single – Predictions for a single value that you specify\.

Use the following procedure to make batch predictions\.

To make batch predictions, choose a dataset and generate predictions for it\.

1. Choose **Select Dataset**\.

1. Choose the dataset\.

1. Choose **Update Prediction**\.

For each set of predictions, SageMaker Canvas returns the following:
+ The predicted values
+ The probability of the predicted value being correct
+ The dataset that you've specified for generating predictions

You can download the model's predictions as a \.csv file\.

The following images visualize the preceding procedure\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/beta-2-pred-1.png)

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/beta-2-pred-2.png)

Use the following procedure to make a prediction for a row in the dataset\.

1. Choose **Single prediction**\.

1. Change the input values to see how the predicted value changes from the average prediction\.

1. Choose **Update Prediction**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/beta-2-pred-3.png)