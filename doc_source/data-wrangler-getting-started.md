# Get Started with Data Wrangler<a name="data-wrangler-getting-started"></a>

Amazon SageMaker Data Wrangler is a feature in Amazon SageMaker Studio\. Use this section to learn how to access and get started using Data Wrangler\. Do the following:

1. Complete each step in [Prerequisites](#data-wrangler-getting-started-prerequisite)\.

1. Follow the procedure in [Access Data Wrangler](#data-wrangler-getting-started-access) to start using Data Wrangler\.

## Prerequisites<a name="data-wrangler-getting-started-prerequisite"></a>

To use Data Wrangler, you must complete the following prerequisites\. 

1. To use Data Wrangler, you need access to an [m5\.4xlarge](https://aws.amazon.com/ec2/instance-types/m5/) Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. To learn how to view your quotas and, if necessary, request a quota increase, see [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.

1. Configure the required permissions described in [Security and Permissions](data-wrangler-security.md)\. 

To use Data Wrangler, you need an active Studio instance\. To learn how to launch a new instance, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\. When your Studio instance is **Ready**, use the instructions in [Access Data Wrangler](#data-wrangler-getting-started-access)\.

## Access Data Wrangler<a name="data-wrangler-getting-started-access"></a>

The following procedure assumes you have completed the [Prerequisites](#data-wrangler-getting-started-prerequisite)\.

**To access Data Wrangler in Studio:**

1. Next to the user you want to use to launch Studio, select **Open Studio**\.

1. When Studio opens, select the **\+** sign on the **New data flow** card under **ML tasks and components**\. This creates a new directory in Studio with a \.flow file inside, which contains your data flow\. The \.flow file automatically opens in Studio\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/open-flow.png)

   You can also create a new flow by selecting **File**, then **New**, and choosing **Data Wrangler Flow** in the top navigation bar\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/new-flow-file-menu.png)

1. \(Optional\) Rename the new directory and the \.flow file\. 

1. When you create a new \.flow file in Studio, you may see a message within the **Data Wrangler** interface that says:

   **Data Wrangler is loading\.\.\.**

   **This may take a few minutes\.**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/establishing-connection-engine.png)

   This message persists as long as the **KernelGateway** app on your **User Details** page is **Pending**\. To see the status of this app, in the SageMaker console on the **Amazon SageMaker Studio** page, select the name of the user you are using to access Studio\. On the **User Details** page, you see a **KernelGateway** app under **Apps**\. Wait until this app status is **Ready** to start using Data Wrangler\. This can take around 5 minutes the first time you launch Data Wrangler\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/gatewayKernel-ready.png)

1. To get started, choose a data source and use it to import a dataset\. See [Import](data-wrangler-import.md) to learn more\. 

   When you import a dataset, it appears in your data flow\. To learn more, see [Create and Use a Data Wrangler Flow](data-wrangler-data-flow.md)\.

1. After you import a dataset, Data Wrangler automatically infers the type of data in each column\. Choose **\+** next to the **Data types** step and select **Edit data types**\. 
**Important**  
After you add transforms to the **Data types** step, you cannot bulk\-update column types using **Update types**\. 

1. Use the data flow to add transforms and analyses\. To learn more see [Transform Data](data-wrangler-transform.md) and [Analyze and Visualize](data-wrangler-analyses.md)\.

1. To export a complete data flow, choose **Export** and choose an export option\. To learn more, see [Export](data-wrangler-data-export.md)\. 

1. Finally, choose the **Components and registries** icon, and select **Data Wrangler** from the dropdown list to see all the \.flow files that you've created\. You can use this menu to find and move between data flows\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/data-wrangler-components.png)

After you have launched Data Wrangler, you can use the following section to walk through how you might use Data Wrangler to create an ML data prep flow\. 

## Update Data Wrangler<a name="data-wrangler-update"></a>

We recommend that you periodically update the Data Wrangler Studio app to access the latest features and updates\. The Data Wrangler app name starts with **sagemaker\-data\-wrang**\. To learn how to update a Studio app, see [Shut down and Update Studio Apps](studio-tasks-update-apps.md)\.

## Demo: Data Wrangler Titanic Dataset Walkthrough<a name="data-wrangler-getting-started-demo"></a>

The following sections provide a walkthrough to help you get started using Data Wrangler\. This walkthrough assumes that you have already followed the steps in [Access Data Wrangler](#data-wrangler-getting-started-access) and have a new data flow file open that you intend to use for the demo\. You may want to rename this \.flow file to something similar to **titanic\-demo\.flow**\.

This walkthrough uses the [Titanic dataset](https://www.openml.org/d/40945)\. This data set contains the survival status, age, gender, and class \(which serves as a proxy for economic status\) of passengers aboard the maiden voyage of the *RMS Titanic* in 1912\.

In this tutorial, you perform the following steps\.
+ Upload the [Titanic dataset](https://www.openml.org/d/40945) to Amazon Simple Storage Service \(Amazon S3\), and then import this dataset into Data Wrangler\.
+ Analyze this dataset using Data Wrangler analyses\. 
+ Define a data flow using Data Wrangler data transforms\.
+ Export your flow to a Jupyter Notebook that you can use to create a Data Wrangler job\. 
+ Process your data, and kick off a SageMaker training job to train a XGBoost Binary Classifier\. 

### Upload Dataset to S3 and Import<a name="data-wrangler-getting-started-demo-import"></a>

To get started, download the [Titanic dataset](https://www.openml.org/d/40945) and upload it to an Amazon S3 \(Amazon S3\) bucket in the AWS Region in which you want to complete this demo\. 

If you are a new user of Amazon S3, you can do this using drag and drop in the Amazon S3 console\. To learn how, see [Uploading Files and Folders by Using Drag and Drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html#upload-objects-by-drag-and-drop) in the Amazon Simple Storage Service User Guide\.

**Important**  
Upload your dataset to an S3 bucket in the same AWS Region you want to use to complete this demo\. 

When your dataset has been successfully uploaded to Amazon S3, it you can import it into Data Wrangler\. 

**Import the Titanic dataset to Data Wrangler**

1. Select the **Import** tab in your Data Wrangler flow file\. 

1. Select **Amazon S3**\. 

1. Use the **Import a dataset from S3** table to find the bucket to which you added the Titanic dataset\. Choose the Titanic dataset CSV file to open the **Details** pane\.

1. Under **Details**, the **File type** should be CSV\. Choose **Add header to table** to specify that the first row of the dataset is a header\. You can also name the dataset something more friendly, such as **Titanic\-train**\.

1. Select **Import dataset**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-titanic-dataset.png)

When your dataset is imported into Data Wrangler, it appears in your data flow\. You can view your data flow at any time by selecting the **Data Flow** tab\. You can double click on a node to enter the node detail view, which allows you to add transformations or analysis; otherwise, you can use the plus icon to navigate for a quick navigation\. In the next section, you use this data flow to add analysis and transform steps\. 

### Data Flow<a name="data-wrangler-getting-started-demo-data-flow"></a>

In the data flow section, the only steps in the data flow are your recently imported dataset and a **Data type** step\. After applying transformations, you can come back to this tab see what the data flow looks like\. Now, add some basic transformations under the **Prepare** and **Analyze** tabs\. 

#### Prepare and Visualize<a name="data-wrangler-getting-started-demo-prep-visualize"></a>

Data Wrangler has built\-in transformations and visualizations that you can use to analyze, clean, and transform your data\. 

The **Data** tab of the node detail view lists all built\-in transformations in the right panel, which also contains an area in which you can add custom transformations\. The following use case showcases how to use these transformations\.

##### Data Exploration<a name="data-wrangler-getting-started-demo-explore"></a>

First, create a table summary of the data using an analysis\. Do the following:

1. Choose the **\+** next to the **Data type** step in your data flow and select **Add analysis**\.

1. In the **Analysis** area, select **Table summary** from the dropdown list\.

1. Give the table summary a **Name**\.

1. Select **Preview** to preview the table that will be created\.

1. Choose **Create** to save it to your data flow\. It appears under **All Analyses**\.

Using the statistics you see, you can make observations similar to the following about this dataset: 
+ Fare average \(mean\) is around $33, while the max is over $500\. This column likely has outliers\. 
+ This dataset uses *?* to indicate missing values\. A number of columns have missing values: *cabin*, *embarked*, and *home\.dest*
+ The age category is missing over 250 values\.

Choose **Prepare** to go back to the data flow\. Next, clean your data using the insights gained from these stats\. 

##### Drop Unused Columns<a name="data-wrangler-getting-started-demo-drop-unused"></a>

Using the analysis from the previous section, clean up the dataset to prepare it for training\. To add a new transform to your data flow, choose **\+** next to the **Data type** step in your data flow and choose **Add transform**\.

First, drop columns that you don't want to use for training\. You can use [Pandas](https://pandas.pydata.org/) data analysis library to do this, or you can use one of the built\-in transforms\. 

To do this using Pandas, follow these steps\.

1. In the **Custom Transform** section, select **Python \(Pandas\)** from the dropdown list\.

1. Enter the following in the code box\.

   ```
   cols = ['name', 'ticket', 'cabin', 'sibsp', 'parch', 'home.dest','boat', 'body']
   df = df.drop(cols, axis=1)
   ```

1. Choose **Preview** to preview the change and then choose **Add** to add the transformation\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/preview-drop.png)

To use the built\-in transformations, do the following: 

1. Choose **Manage columns** from the right panel\.

1. For **Input column**, choose **cabin**, and choose **Preview**\.

1. Verify that the **cabin** column has been dropped, then choose **Add**\.

1. Repeat these steps for the following columns: **ticket**, **name**, **sibsp**, **parch**, **home\.dest**, **boat**, and **body**\. 

##### Clean up Missing Values<a name="data-wrangler-getting-started-demo-missing-vals"></a>

Now, clean up missing values\. You can do this with the **Handling missing values** transform group\.

A number of columns have missing values\. Of the remaining columns, *age* and *fare* contain missing values\. Inspect this using the **Custom Transform**\. 

Using the **Python \(Pandas\)** option, use the following to quickly review the number of entries in each column:

```
df.info()
```

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/inspect-missing-pandas.png)

To drop rows with missing values in the *age* category, do the following: 

1. Choose **Handling missing values**\. 

1. Choose **Drop missing** for the **Transformer**\.

1. Choose **Drop Rows** for the **Dimension**\.

1. Choose *age* for the **Input column**\.

1. Choose **Preview** to see the new data frame, and then choose **Add** to add the transform to your flow\.

1. Repeat the same process for *fare*\. 

You can use `df.info()` in the **Custom transform** section to confirm that all rows now have 1,045 values\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/inspect-after-drop.png)

##### Custom Pandas: Encode<a name="data-wrangler-getting-started-demo-encode"></a>

Try flat encoding using Pandas\. Encoding categorical data is the process of creating a numerical representation for categories\. For example, if your categories are `Dog` and `Cat`, you may encode this information into two vectors: `[1,0]` to represent `Dog`, and `[0,1]` to represent `Cat`\.

1. In the **Custom Transform** section, choose **Python \(Pandas\)** from the dropdown list\.

1. Enter the following in the code box\.

   ```
   import pandas as pd
   
   dummies = []
   cols = ['pclass','sex','embarked']
   for col in cols:
       dummies.append(pd.get_dummies(df[col]))
       
   encoded = pd.concat(dummies, axis=1)
   
   df = pd.concat((df, encoded),axis=1)
   ```

1. Choose **Preview** to preview the change\. The encoded version of each column is added to the dataset\. 

1. Choose **Add** to add the transformation\. 

#### Custom SQL: SELECT Columns<a name="data-wrangler-getting-started-demo-sql"></a>

Now, select the columns you want to keep using SQL\. For this demo, select the columns listed in the following `SELECT` statement\. Because *survived* is your target column for training, put that column first\.

1. In the **Custom Transform** section, select **SQL \(PySpark SQL\)** from the dropdown list\.

1. Enter the following in the code box\.

   ```
   SELECT survived, age, fare, 1, 2, 3, female, male, C, Q, S FROM df;
   ```

1. Choose **Preview** to preview the change\. The columns listed in your `SELECT` statement are the only remaining columns\.

1. Choose **Add** to add the transformation\. 

### Export<a name="data-wrangler-getting-started-export"></a>

When you've finished creating a data flow, you have a number of export options\. The following section explains how to export to a Data Wrangler job notebook\. A Data Wrangler job is used to process your data using the steps defined in your data flow\. To learn more about all export options, see [Export](data-wrangler-data-export.md)\.

#### Export to Data Wrangler Job Notebook<a name="data-wrangler-getting-started-export-notebook"></a>

When you export your data flow using a **Data Wrangler job**, the process automatically creates a Jupyter Notebook\. This notebook automatically opens in your Studio instance and is configured to run a SageMaker processing job to run your Data Wrangler data flow, which is referred to as a Data Wrangler job\. 

1. Save your data flow\. Select **File** and then select **Save Data Wrangler Flow**\.

1. Choose the **Export** tab\.

1. Select the last step in your data flow\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/export-select-step.png)

1. Choose **Data Wrangler Job**\. This opens a Jupyter Notebook\.

1. Choose any **Python 3 \(Data Science\)** kernel for the **Kernel**\. 

1. When the kernel starts, run the cells in the notebook book until **Kick off SageMaker Training Job \(Optional\)**\. 

1. Optionally, you can run the cells in **Kick off SageMaker Training Job \(Optional\)** if you want to create a SageMaker training job to train an XGboost classifier\. You can find the cost to run an SageMaker training job in [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\. 

   Alternatively, you can add the code blocks found in [Training XGBoost Classifier](#data-wrangler-getting-started-train-xgboost) to the notebook and run them to use the [XGBoost](https://xgboost.readthedocs.io/en/latest/) open source library to train an XGBoost classifier\. 

1. Uncomment and run the cell under **Cleanup** and run it to revert the SageMaker Python SDK to its original version\.

You can monitor your Data Wrangler job status in the SageMaker console in the **Processing** tab\. Additionally, you can monitor your Data Wrangler job using Amazon CloudWatch\. For additional information, see [Monitor Amazon SageMaker Processing Jobs with CloudWatch Logs and Metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html#processing-job-cloudwatch)\. 

If you kicked off a training job, you can monitor its status using the SageMaker console under **Training jobs** in the **Training section**\.

#### Training XGBoost Classifier<a name="data-wrangler-getting-started-train-xgboost"></a>

In the same notebook that kicked off the Data Wrangler job, you can pull the data and train a XGBoost Binary Classifier using the prepared data with minimal data preparation\. 

1. First, upgrade necessary modules using `pip` and remove the \_SUCCESS file \(this last file is problematic when using `awswrangler`\)\.

   ```
   ! pip install --upgrade awscli awswrangler boto sklearn
   ! aws s3 rm {output_path} --recursive  --exclude "*" --include "*_SUCCESS*"
   ```

1. Read the data from Amazon S3\. You can use `awswrangler` to recursively read all the CSV files in the S3 prefix\. The data is then split into features and labels\. The label is the first column of the dataframe\.

   ```
   import awswrangler as wr
   
   df = wr.s3.read_csv(path=output_path, dataset=True)
   X, y = df.iloc[:,:-1],df.iloc[:,-1]
   ```
   + Finally, create DMatrices \(the XGBoost primitive structure for data\) and do cross\-validation using the XGBoost binary classification\.

     ```
     import xgboost as xgb
     
     dmatrix = xgb.DMatrix(data=X, label=y)
     
     params = {"objective":"binary:logistic",'learning_rate': 0.1, 'max_depth': 5, 'alpha': 10}
     
     xgb.cv(
         dtrain=dmatrix, 
         params=params, 
         nfold=3,
         num_boost_round=50,
         early_stopping_rounds=10,
         metrics="rmse", 
         as_pandas=True, 
         seed=123)
     ```

#### Shut down Data Wrangler<a name="data-wrangler-getting-started-shut-down"></a>

When you are finished using Data Wrangler, we recommend that you shut down the instance it runs on to avoid incurring additional charges\. To learn how to shut down the Data Wrangler app and associated instance, see [Shut Down Data Wrangler](data-wrangler-shut-down.md)\. 