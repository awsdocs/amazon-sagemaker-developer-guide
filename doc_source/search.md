# Manage Machine Learning Experiments with Search<a name="search"></a>


|  | 
| --- |
| Search is in preview release for Amazon SageMaker and is subject to change\. | 

To find, organize, and evaluate experiments, use Amazon SageMaker search\. Search can help you to manage your training jobs, models and resources\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/search-1.png)

Amazon SageMaker search is ideal for:
+ Finding, organizing, or evaluating training jobs using properties, hyperparameters, or any other metadata
+ Using a search result to rank training jobs and models based on metrics, such as training loss or validation accuracy to find the best performing one
+ Tracing the lineage of a model back to the training job and its related resources, such as the training datasets

**Topics**
+ [Use Search to Find, Organize, and Evaluate Training Jobs](#search-organize)
+ [Search for Information About Related Resources](#search-lineage)

## Use Search to Find, Organize, and Evaluate Training Jobs<a name="search-organize"></a>

To find a specific training job, model, or resource, use Amazon SageMaker search as you would any search engine\. Amazon SageMaker searches for the keyword in all searchable items, which include training jobs, models, hyperparameters, metadata, tags, and URLs\. You can use search with tags to help organize your training jobs\. To help refine your search results, you can search using multiple search criteria\.

  To create a model, you need to experiment\. When you perform an experiment, you might test a different algorithm, tune a hyperparameter, or use a different dataset\. You make changes in steps, then study their effect on the model’s performance\. Eventually, you need to evaluate all of your models against one or several metrics and choose the best one\. You can use a search result to list models and evaluate them\. 

### Find Training Jobs \(Console\)<a name="search-organize-console"></a>

**To use search \(console\)**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker](https://console.aws.amazon.com/sagemaker)\.

1. In the navigation pane, choose **Search**\.

1. For **Resource type**, choose **Training jobs**\.

1. To add a parameter, for **Parameters**, provide the following:

   1. Enter a parameter in the search box and choose a parameter type, for example **TrainingJobName**\.

   1. Choose the conditional operation to use\. This sets the condition that values must satisfy to be included in a search result\. For numeral values, use operators such as **is equals to**, **lesser than**, or **or greater than**\. For text\-based values use operators, such as **equals to** or **contains**\.

   1. Enter the value for the parameter\.

1. \(optional\) To refine your search, add additional search criteria, choose **Add parameter** and enter the parameter values\. When you provide multiple parameters, Amazon SageMaker includes all parameters in the search\.

1. Choose **Search**\.

#### Use Tags to Search for Training Jobs<a name="search-tags-console"></a>

You can use tags as search criteria\. To group training jobs, create tags with descriptive keys and value\. For example, create tag keys for: project, owner, customer, and industry\. 

**To use tags in a search**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker](https://console.aws.amazon.com/sagemaker)\.

1.  In the navigation pane, choose **Search**\. 

1. For **Tags**, enter a tag key and a tag value\.

1. To add more tags to the search, choose **Add tag** and add a another tag key\-value pair for each new tag that you want to add\.

When you use tags in a search, in the results, the key is a column name and the values are entries in rows\.

#### Evaluate Models Returned by a Search<a name="search-evaluate-console"></a>

To evaluate different models, find their metrics with a search\. To highlight metrics, adjust the view to show only metrics and important hyperparameters\. 

**To change the viewable metadata, hyperparameters, and metrics:**

1. After performing a search, at the search results table, choose the cog icon to show the preferences window\.

1. To show or hide a **Hyperparameter** or **Metric**, use its toggle switch\.

1. To update the view after making changes, choose **Update view**\.

After viewing metrics and important hyperparameters, you can compare and contrast the result\. From there, you can choose the best model to host or investigate the models that are performing poorly\.

### Use Search to Find Training Jobs \(API\)<a name="search-api"></a>

 To use search, create a search parameter, then use the search function found in `smclient` of the low\-level SDK for Python \(Boto 3\)\. 

The following example shows how to use search using the API: 

```
import boto3

search_params={
   "MaxResults": 10,
   "Resource": "TrainingJob",
   "SearchExpression": { 
      "Filters": [{ 
            "Name": "Tags.Project",
            "Operator": "Equals",
            "Value": "Project_Binary_Classifier"
         }]},
  "SortBy": "Metrics.train:binary_classification_accuracy",
  "SortOrder": "Descending"
}

smclient = boto3.client(service_name='sagemaker')
results = smclient.search(**search_params)
```

### Evaluate Models \(API\)<a name="search-organize-api"></a>

To evaluate different models, see the metrics in a search result\. To evaluate models using the low\-level SDK for Python \(Boto 3\), create a table and plot it\.

The following example shows how to use search to evaluate models and show the results in a table:

```
import pandas

headers=["Training Job Name", "Training Job Status", "Batch Size", "Binary Classification Accuracy"]
rows=[]
for result in results['Results']: 
    trainingJob = result['TrainingJob']
    metrics = trainingJob['FinalMetricDataList']
    rows.append([trainingJob['TrainingJobName'],
     trainingJob['TrainingJobStatus'],
     trainingJob['HyperParameters']['mini_batch_size'],
     metrics[[x['MetricName'] for x in  
     metrics].index('train:binary_classification_accuracy')]['Value']
    ])

df = pandas.DataFrame(data=rows,columns=headers)

from IPython.display import display, HTMLdisplay(HTML(df.to_html()))
```

### Get Suggestions for a Search \(API\)<a name="search-suggestion-api"></a>

To get suggestions for a search, use `get_search_suggestions` function in the API\.

The following code example shows a `get_search_suggestions` request syntax:

```
search_suggestion_params={
   "Resource": "TrainingJob",
   "SuggestionQuery": { 
      "PropertyNameQuery": { 
         "PropertyNameHint": "linear"
      }
   }
}
```

The following code example shows an expected `get_search_suggestions` response syntax:

```
{
        'PropertyNameSuggestions': [{'PropertyName': 'hyperparameters.linear_init_method'},
                    {'PropertyName': 'hyperparameters.linear_init_value'},
                    {'PropertyName': 'hyperparameters.linear_init_sigma'},
                    {'PropertyName': 'hyperparameters.linear_lr'},
                    {'PropertyName': 'hyperparameters.linear_wd'}]
}
```

After you get the search suggestions, you can use one of the property names in a search\.

### Verify the Contents of Your Training Jobs<a name="search-verify"></a>

You can use Amazon SageMaker search to verify which datasets were used in training, where the holdout datasets were used, and other details about training jobs\. For example, if you need to verify that a dataset was used in a training job for an audit or to verify compliance, use Amazon SageMaker search\.

To check if a holdout dataset or any other dataset was used in a training job, search for its Amazon S3 URL\. Amazon SageMaker uses the dataset as a search parameter and lists the training jobs that used the dataset\. If your search result is empty, it proves that the dataset was not used in a training job\.

## Search for Information About Related Resources<a name="search-lineage"></a>

You can use Amazon SageMaker search to see information about training jobs and the related model or resources\. For example, if you find that a hosted model's performance has declined, you can review its training job and the resources used to determine what's causing the problem\.

There are three types of investigations:
+ You can investigate a model, navigate to review the details for the training job, and then review the job's dataset, algorithm, hyperparameters, and metrics\.
+ You can investigate a dataset, then inspect the training job and its related settings, investigate the model, and finally investigate the endpoint\.
+ You can investigate a model's containers\. A model might be composed of a single model container or multiple model containers\. A model made of multiple model containers is called a *pipeline model\.* Each model container is a distinct model that contains links to the training job, dataset, and associated environment variables\.

**Note**  
In Amazon SageMaker, a pipeline model can have up to three model containers\. These pipelines also support the deployment of a linear sequence of pretrained model containers, where the output of one machine learning model provides the input to the next\. A pipeline model is useful for solving classification problems, where each supervised model is trained using a specific subset of a dataset that is designated as a class\. When generating inference on a pipeline model, you get results from each model container\.

### Search for Information About Related Resources \(Console\)<a name="search-lineage-console"></a>

To see a list of URLs to a model or training job's resources, see the details page for the model or training job\. To access the details page, choose **Training jobs** or **Models** in the navigation pane\.

### Search for Information About Related Resources \(API\)<a name="search-lineage-api"></a>

To trace a model's lineage, you need to obtain the model's name, then use it to search for training jobs\.

The following example shows how to use search to trace a model's lineage using the API:

```
# Get the name of model deployed at endpoint
endpoint_config = smclient.describe_endpoint_config(EndpointConfigName=endpointName)
model_name = endpoint_config['ProductionVariants'][0]['ModelName']

# Get the model's name
model = smclient.describe_model(ModelName=model_name)

# Search the training job by Amazon S3 location of model artifacts
search_params={
   "MaxResults": 1,
   "Resource": "TrainingJob",
   "SearchExpression": { 
      "Filters": [ 
         { 
            "Name": "ModelArtifacts.S3ModelArtifacts",
            "Operator": "Equals",
            "Value": model['PrimaryContainer']['ModelDataUrl']
         }]},
}
results = smclient.search(**search_params)
```

After you find your training job, you can investigate the resources used to train the model\.