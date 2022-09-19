# Create an Amazon SageMaker Autopilot experiment<a name="autopilot-automate-model-development-create-experiment"></a>

Using Amazon SageMaker Autopilot to create an experiment is an easy way to get started with machine learning\. You can use a user interface \(UI\) to help you populate the input, output, target and parameters to run and evaluate an Autopilot experiment\. The UI has descriptions, toggle switches, drop down menus, radio buttons and more to help you navigate creating your model\.

You can view statistics while the experiment is running\. After it runs, you can compare trials and delve into the details\.

The following instructions show you how to create an Amazon SageMaker Autopilot experiment\. You will name it, provide locations for the input and output data, and specify which target data to predict\. Optionally, you can also specify the type of machine learning problem that you want to solve\. 

1. Open the Studio console after signing in at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and select Studio from the navigation pane\. Then select the **Launch SageMaker Studio** button\. Next, select **Launch app** in the row containing your user name, and choose Studio from the drop down list\. Last, select the **Build models automatically** center card from the Studio launcher tab\. See the [quick setup guide](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html) for more information about starting Studio for the first time\.

1. A page titled **Create an Autopilot experiment** opens\. The page includes fields for **Experiment and data details** such as name, S3 bucket location, split ratio, and target of the experiment\.   
![\[Studio console's Create an Autopilot experiment page: Includes settings for experiment and data details.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-create-experiment-data-details.PNG)

1. In the **Experiment and data details** section of the **Create an Autopilot experiment** page, enter information about the experiment, as follows:

   1. **Experiment name** – Must be unique to your account in the current AWS Region and contain a maximum of 63 alphanumeric characters\. Can include hyphens \(\-\) but not spaces\.

   1. **Input data** – Provide the S3 bucket location of your input data\. This S3 bucket must be in your current AWS Region\. The URL must be in an s3:// format where Amazon SageMaker has write permissions\. The file must be in CSV or parquet format and contain at least 500 rows\. Select `Browse` to scroll through available paths and `Preview` to see a sample of your input data\.

   1. **Is your S3 input a manifest file?** – A manifest file includes metadata with your input data\. The metadata specifies the location of your data in Amazon S3 storage\. It also specifies how the data is formatted and which attributes from the dataset to use when training your model\. You can use a manifest file as an alternative to preprocessing when your labeled data is being streamed in *Pipe* mode\.

   1. **Target** – The name of the data column to target for model predictions\.

   1. **Auto split data?** – Autopilot can split your data into an 80\-20% split for training and validation data\. If you prefer a custom split, you can choose the **Specify split ratio**\. To use a custom dataset for validation, choose **Provide a validation set**\.

   1. **Output data location \(S3 bucket\)** – The name of the S3 bucket location where you want to store the output data\. The URL for this bucket must be in an S3 format where Amazon SageMaker has write permissions\. The S3 bucket must be in the current AWS Region\. Autopilot can also create this for you in the same location as your input data\. 

   1. Select **Next: Deployment and advanced settings** to open the **Deployment and advanced settings** as shown in the following image\. Settings include auto display endpoint name, machine learning problem type, and choices for running your experiment\.  
![\[SageMaker Studio Deployment and advanced settings for endpoints and machine learning problem type.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-deploy-advanced-settings.PNG)

1. **Automatic deployment will fail if either the default resource quota or your customer quota for endpoint instances in a Region is too limited\.** Currently, you are required to have at least two ml\.m5\.2xlarge instances\. If you encounter a failure related to quotas, you can [request a service limit increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) for SageMaker endpoint instances\.

1. **Deployment settings \- ** Autopilot can automatically create an endpoint and deploy your model for you\. 

   1. To auto deploy to an automatically generated endpoint, or to provide an endpoint name for custom deployment, ensure that the toggle is set to **Yes** under **Auto deploy?** 

   1. **Advanced settings \(optional\)** – Autopilot provides additional controls to manually set experimental parameters\. 

      1. **Machine learning problem type** – Autopilot can automatically select the machine learning problem type\. If you prefer to choose it manually, use the **Select the machine learning problem type** dropdown menu\.

         1. **Auto** – Autopilot infers the problem type from the values of the attribute that you want to predict\. In some cases, SageMaker is unable to infer accurately\. When that happens, you must provide the value for the job to succeed\.

         1. **Binary classification **– Binary classification is a type of supervised learning that assigns an individual to one of two predefined, and mutually exclusive classes, based on their attributes\. For example, medical diagnosis based on results of diagnostic tests that determine if someone has a disease\.

         1. **Regression** – Regression estimates the values of a dependent target variable based on one or more variables or attributes that are correlated with it\. For example, house prices based on features, such as square footage and number of bathrooms\.

         1. **Multiclass classification** – Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. For example, the prediction of the topic most relevant to a text document, such as politics, finance, or philosophy\.

      1. **Choose how to run your experiment** – You can specify how to run your experiment as either a complete experiment or as a pilot\.

         1. If you choose **Run a complete experiment**, Autopilot generates a model and statistics that you can view in real time while the experiment is running\. 

         1. If you choose **Run a pilot notebook to create a notebook with candidate definitions**, instead of running the entire workflow, Autopilot stops running after a candidate definition notebook is generated\. A candidate is a combination of data preprocessors, algorithms, and algorithm parameter settings\. You can use the notebook as a starting point to guide your own process of model training/tuning\. The notebook has highlighted sections that explain which changes are typical, such as changing instance type and cluster size\. 

            After the experiment is complete, you can view the trials, sort by objective metric, and deploy the model for use in other environments\. 

      1. Select **Next: Review and create** to obtain a summary of your Autopilot experiment prior to creation\. 

1. Select **Create experiment**\. Autopilot provides status on the course of the experiment, a list of generated models, and the job profile used to create them\.

**Note**  
To avoid incurring unnecessary charges: If you deploy a model that is no longer needed, delete the endpoints and resources that were created during that deployment\. Information about pricing instances by Region is available at [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.