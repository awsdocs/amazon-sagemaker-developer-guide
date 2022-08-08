# Build a model<a name="canvas-build-model"></a>

Use Amazon SageMaker Canvas to build a model on the dataset that you've imported\. Use the model that you've built to make predictions on new data\. SageMaker Canvas uses the information in the dataset to build up to 250 models and choose the one that performs the best\.

For each model that you build, you choose the **Target column**\. The **Target column** is the column that contains the information that you want to predict\. For example, if you're building a model to predict whether people have cancelled their subscriptions, the **Target column** contains data points that are either a "yes" or a "no" about someone's cancellation status\.

Amazon SageMaker Canvas uses the data in the **Target column** to automatically recommend one or more **Model types**\. Model types fall into one of the following categories:
+ Categorical prediction, known as *classification* in machine learning
+ Numeric prediction, known as *regression* in machine learning

The following are the types of categorical prediction:
+ *2 category prediction*\. The machine learning term for 2 category prediction is *binary classification*\. Predicting whether someone has cancelled their subscription is an example of 2 category prediction\.
+ *3\+ category prediction*\. The machine learning term for 3\+ category prediction is *multiclass classification*\.

Amazon SageMaker Canvas predicts the value of the **Target column** by using the information in the rest of the dataset\. For categorical prediction, SageMaker Canvas puts each row into one of the categories listed in the **Target column**\. For numeric prediction, SageMaker Canvas uses the information in the dataset to predict the numeric values in the **Target column**\.

The **Build** page of SageMaker Canvas generates a preview of 100 rows taken from your dataset, or if your dataset has more than 20,000 rows, then SageMaker Canvas selects 100 rows from a random sample of your dataset\. To learn more about the random sample and how you can change the sample size, see the following section [Random sample](#canvas-random-sample)\.

Before building your model, you can filter your data or prepare it using advanced transforms\. For more information about preparing your data for model building, see [Prepare data with advanced transformations](canvas-prepare-data.md)\.

To build your model, you can choose either a **Quick build** or a **Standard build**\. The **Quick build** usually takes 2\-15 minutes to build the model, whereas the **Standard build** usually takes 2\-4 hours and generally has a higher accuracy\. For a **Quick build**, your input dataset can have a maximum of 50,000 rows\. If you log out while running a **Quick build**, your build might be interrupted until you log in again\. When you log in again, SageMaker Canvas restarts the **Quick build**\.

While Amazon SageMaker Canvas builds the model, it automatically adds missing values for datasets that don't have time series data\. SageMaker Canvas uses the values in your dataset to perform a mathematical approximation for the missing values\. For the highest model accuracy, we recommend adding in the missing data if you can find it\.

Amazon SageMaker Canvas can make time series forecasts on your data\. Time series forecasts are useful for when you make predictions over a period of time\. For information about time series forecasts, see [Time Series Forecasts in Amazon SageMaker Canvas](canvas-time-series.md)\.

## Preview a model<a name="canvas-preview-model"></a>

Amazon SageMaker Canvas gives you the ability to get insights from your data before you build a model by choosing **Preview model**\. For example, you can see how the data in each column is distributed\. For models built using categorical data, you can also choose **Preview model** to generate an **Estimated accuracy** prediction of how well the model might analyze your data\. The accuracy of a **Quick build** or a **Standard build** represents how well the model can perform on real data and is generally higher than the **Estimated accuracy**\.

Amazon SageMaker Canvas automatically handles missing values in your dataset while it builds the model\. It infers the missing values by using adjacent values that are present in the dataset\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-build/canvas-build-preview-model.png)

## Random sample<a name="canvas-random-sample"></a>

SageMaker Canvas uses the random sampling method to sample your dataset\. The random sample method means that each row has an equal chance of being picked for the sample\. You can choose a column in the preview to get summary statistics for the random sample, such as the mean and the mode\.

By default, SageMaker Canvas uses a random sample size of 20,000 rows from your dataset for datasets with more than 20,000 rows\. For datasets smaller than 20,000 rows, the default sample size is the number of rows in your dataset\. You can increase or decrease the sample size by choosing **Random sample** in the **Build** tab of the SageMaker Canvas app\. You can use the slider to select your desired sample size, and then choose **Update** to change the sample size\. The maximum sample size you can choose for a dataset is 40,000 rows, and the minimum sample size is 500 rows\. If you choose a large sample size, the dataset preview and summary statistics might take a few moments to reload\.

The **Build** page shows a preview of 100 rows from your dataset\. If the sample size is the same size as your dataset, then the preview uses the first 100 rows of your dataset\. Otherwise, the preview uses the first 100 rows of the random sample\.