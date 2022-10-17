# Create an Amazon SageMaker Autopilot experiment<a name="autopilot-automate-model-development-create-experiment"></a>

This guide shows how to create an Amazon SageMaker Autopilot experiment, so that you can analyze data, and create a notebook with candidate model definitions\. This can help you get started with machine learning quickly\.

You can use a user interface \(UI\) to help you populate the input, output, target, and parameters to run and evaluate an Autopilot experiment\. The UI has descriptions, toggle switches, drop down menus, radio buttons and more to help you navigate creating your model\. You can also view statistics while the experiment is running\. After it runs, you can compare trials and delve into the details\.

The following instructions show how to create an Amazon SageMaker Autopilot job as a pilot experiment\. You will name your experiment, provide locations for the input and output data, and specify which target data to predict\. Optionally, you can also specify the type of machine learning problem that you want to solve\. 

**To create an Autopilot experiment**

1. Sign in at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and select **Studio** from the navigation pane\. 

1. When the SageMaker Studio console opens, choose the **Launch SageMaker Studio** button\. 

1. Next, select **Launch app** in the row containing your user name, and choose **Studio** from the dropdown list\. 

1. Select the **Build models automatically** center card from the **Studio launcher** tab\. See the [quick setup guide](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html) for more information about starting Studio for the first time\.

1. A page titled **Create an Autopilot experiment** opens\. The page includes fields for **Experiment and data details** such as name, Amazon S3 bucket location, split ratio, and target of the experiment\. 

1. In the **Experiment and data details** section of the **Create an Autopilot experiment** page, enter the following information:

   1. **Experiment name** – Must be unique to your account in the current AWS Region and contain a maximum of 63 alphanumeric characters\. Can include hyphens \(\-\) but not spaces\.

   1. **Input data** – Provide the S3 bucket location of your input data\. This S3 bucket must be in your current AWS Region\. The URL must be in an `s3://` format where Amazon SageMaker has write permissions\. The file must be in CSV or parquet format and contain at least 500 rows\. Select `Browse` to scroll through available paths and `Preview` to see a sample of your input data\.

   1. **Is your S3 input a manifest file?** – A manifest file includes metadata with your input data\. The metadata specifies the location of your data in Amazon Simple Storage Service \(Amazon S3\)\. It also specifies how the data is formatted and which attributes from the dataset to use when training your model\. You can use a manifest file as an alternative to preprocessing when your labeled data is being streamed in *Pipe* mode\.

   1. **Auto split data?** – Autopilot can split your data into an 80\-20% split for training and validation data\. If you prefer a custom split, you can choose the **Specify split ratio**\. To use a custom dataset for validation, choose **Provide a validation set**\.

   1. **Output data location \(S3 bucket\)** – The name of the S3 bucket location where you want to store the output data\. The URL for this bucket must be in an Amazon S3 format where Amazon SageMaker has write permissions\. The S3 bucket must be in the current AWS Region\. Autopilot can also create this for you in the same location as your input data\. 

1. Select **Next: Target and features**\. The **Target and features** tab opens\.

1. In the **Target and features** section, select a column to set as a target for model predictions\. You can also select features for training and change their data type\. The following data types are available: `Text`, `Numerical`, `Categorical`, `Datetime`, `Sequence` and `Auto`\. All features are selected by default\.

1. Select **Next: Training method**\. The **Training method** tab opens\.

1. In the **Training method** section, select from the following training options: **Ensembling**, **Hyperparameter optimization \(HPO\)**, or let Autopilot choose it automatically based on the dataset size\.

   For more information about these training modes, see the **Autopilot training modes** section in the [Training modes and algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-model-support-validation.html) page\.

1. Select **Next: Deployment and advanced settings** to open the **Deployment and advanced settings**\. Settings include auto display endpoint name, machine learning problem type, and choices for running your experiment\.

1. Currently, you are required to have at least two ml\.m5\.2xlarge instances\. 

   Automatic deployment will fail for either of these reasons: 
   + The default resource quota for endpoint instances in a Region exceeds the limit\.
   + The customer quota for endpoint instances in a Region exceeds the limit\.

   If you encounter a failure related to quotas, you can [request a service limit increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) for SageMaker endpoint instances\.

1. **Deployment settings** – Autopilot can automatically create an endpoint and deploy your model for you\. 

   1. To auto deploy to an automatically generated endpoint, or to provide an endpoint name for custom deployment, set the toggle to **Yes** under **Auto deploy?** If you are importing data from Amazon SageMaker Data Wrangler, you have additional options to auto deploy the best model with or without the transforms from Data Wrangler\.
**Note**  
If your Data Wrangler flow contains multi\-row operations such as `groupby`, `join` or `concatenate`, you won't be able to auto deploy with these transforms\. For more information see [Automatically Train Models on Your Data Flow](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-autopilot.html)\.

   1. **Advanced settings \(optional\)** – Autopilot provides additional controls to manually set experimental parameters\. 

      1. **Machine learning problem type** – Autopilot can automatically select the machine learning problem type\. If you prefer to choose it manually, use the **Select the machine learning problem type** dropdown menu\.

         1. **Auto** – Autopilot infers the problem type from the values of the attribute that you want to predict\. In some cases, SageMaker is unable to infer accurately\. When that happens, you must provide the value for the job to succeed\.

         1. **Binary classification**– Binary classification is a type of supervised learning that assigns an individual to one of two predefined and mutually exclusive classes, based on their attributes\. For example, medical diagnosis based on results of diagnostic tests that determine if someone has a disease\.

         1. **Regression** – Regression estimates the values of a dependent target variable based on one or more variables or attributes that are correlated with it\. For example, house prices based on features, such as square footage and number of bathrooms\.

         1. **Multiclass classification** – Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. For example, the prediction of the topic most relevant to a text document, such as politics, finance, or philosophy\.

      1. Select **Next: Review and create** to get a summary of your Autopilot experiment before you create it\. 

1. Select **Create experiment**\. Autopilot provides status on the course of the experiment, a list of generated models, and the job profile used to create them\.

**Note**  
To avoid incurring unnecessary charges: If you deploy a model that is no longer needed, delete the endpoints and resources that were created during that deployment\. Information about pricing instances by Region is available at [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.