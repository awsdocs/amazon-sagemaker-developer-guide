# XGBoost Release 0\.72<a name="xgboost-72"></a>

This previous release of the Amazon SageMaker XGBoost algorithm is based on the 0\.72 release\. [XGBoost](https://github.com/dmlc/xgboost) \(eXtreme Gradient Boosting\) is a popular and efficient open\-source implementation of the gradient boosted trees algorithm\. Gradient boosting is a supervised learning algorithm that attempts to accurately predict a target variable by combining the estimates of a set of simpler, weaker models\. XGBoost has done remarkably well in machine learning competitions because it robustly handles a variety of data types, relationships, and distributions, and because of the large number of hyperparameters that can be tweaked and tuned for improved fits\. This flexibility makes XGBoost a solid choice for problems in regression, classification \(binary and multiclass\), and ranking\.

Customers should consider using the new release of [XGBoost Algorithm](xgboost.md)\. They can use it as a SageMaker built\-in algorithm or as a framework to run scripts in their local environments as they would typically, for example, do with a Tensorflow deep learning framework\. The new implementation has a smaller memory footprint, better logging, improved hyperparameter validation, and an expanded set of metrics\. The earlier implementation of XGBoost remains available to customers if they need to postpone migrating to the new version\. But this previous implementation will remain tied to the 0\.72 release of XGBoost\.

**Topics**
+ [Input/Output Interface for the XGBoost Release 0\.72](#xgboost-72-InputOutput)
+ [EC2 Instance Recommendation for the XGBoost Release 0\.72](#xgboost-72-Instance)
+ [XGBoost Release 0\.72 Sample Notebooks](#xgboost-72-sample-notebooks)
+ [XGBoost Release 0\.72 Hyperparameters](#xgboost-72-hyperparameters)
+ [Tune an XGBoost Release 0\.72 Model](#xgboost-72-tuning)

## Input/Output Interface for the XGBoost Release 0\.72<a name="xgboost-72-InputOutput"></a>

Gradient boosting operates on tabular data, with the rows representing observations, one column representing the target variable or label, and the remaining columns representing features\. 

The SageMaker implementation of XGBoost supports CSV and libsvm formats for training and inference:
+ For Training ContentType, valid inputs are *text/libsvm* \(default\) or *text/csv*\.
+ For Inference ContentType, valid inputs are *text/libsvm* or \(the default\) *text/csv*\.

**Note**  
For CSV training, the algorithm assumes that the target variable is in the first column and that the CSV does not have a header record\. For CSV inference, the algorithm assumes that CSV input does not have the label column\.   
For libsvm training, the algorithm assumes that the label is in the first column\. Subsequent columns contain the zero\-based index value pairs for features\. So each row has the format: <label> <index0>:<value0> <index1>:<value1> \.\.\. Inference requests for libsvm may or may not have labels in the libsvm format\.

This differs from other SageMaker algorithms, which use the protobuf training input format to maintain greater consistency with standard XGBoost data formats\.

For CSV training input mode, the total memory available to the algorithm \(Instance Count \* the memory available in the `InstanceType`\) must be able to hold the training dataset\. For libsvm training input mode, it's not required, but we recommend it\.

SageMaker XGBoost uses the Python pickle module to serialize/deserialize the model, which can be used for saving/loading the model\.

**To use a model trained with SageMaker XGBoost in open source XGBoost**
+ Use the following Python code:

  ```
  import pickle as pkl 
  import tarfile
  import xgboost
  
  t = tarfile.open('model.tar.gz', 'r:gz')
  t.extractall()
  
  model = pkl.load(open(model_file_path, 'rb'))
  
  # prediction with test data
  pred = model.predict(dtest)
  ```

**To differentiate the importance of labelled data points use Instance Weight Supports**
+ SageMaker XGBoost allows customers to differentiate the importance of labelled data points by assigning each instance a weight value\. For *text/libsvm* input, customers can assign weight values to data instances by attaching them after the labels\. For example, `label:weight idx_0:val_0 idx_1:val_1...`\. For *text/csv* input, customers need to turn on the `csv_weights` flag in the parameters and attach weight values in the column after labels\. For example: `label,weight,val_0,val_1,...`\)\.

## EC2 Instance Recommendation for the XGBoost Release 0\.72<a name="xgboost-72-Instance"></a>

SageMaker XGBoost currently only trains using CPUs\. It is a memory\-bound \(as opposed to compute\-bound\) algorithm\. So, a general\-purpose compute instance \(for example, M4\) is a better choice than a compute\-optimized instance \(for example, C4\)\. Further, we recommend that you have enough total memory in selected instances to hold the training data\. Although it supports the use of disk space to handle data that does not fit into main memory \(the out\-of\-core feature available with the libsvm input mode\), writing cache files onto disk slows the algorithm processing time\.

## XGBoost Release 0\.72 Sample Notebooks<a name="xgboost-72-sample-notebooks"></a>

For a sample notebook that shows how to use the latest version of SageMaker XGBoost as a built\-in algorithm to train and host a regression model, see [Regression with Amazon SageMaker XGBoost algorithm](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_abalone.ipynb)\. To use the 0\.72 version of XGBoost, you need to change the version in the sample code to 0\.72\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The topic modeling example notebooks using the XGBoost algorithms are located in the **Introduction to Amazon algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.

## XGBoost Release 0\.72 Hyperparameters<a name="xgboost-72-hyperparameters"></a>

The following table contains the hyperparameters for the XGBoost algorithm\. These are parameters that are set by users to facilitate the estimation of model parameters from data\. The required hyperparameters that must be set are listed first, in alphabetical order\. The optional hyperparameters that can be set are listed next, also in alphabetical order\. The SageMaker XGBoost algorithm is an implementation of the open\-source XGBoost package\. Currently SageMaker supports version 0\.72\. For more detail about hyperparameter configuration for this version of XGBoost, see [ XGBoost Parameters](https://xgboost.readthedocs.io/en/release_0.72/parameter.html)\.


| Parameter Name | Description | 
| --- | --- | 
| num\_class | The number of classes\. **Required** if `objective` is set to *multi:softmax* or *multi:softprob*\. Valid values: integer  | 
| num\_round | The number of rounds to run the training\. **Required** Valid values: integer  | 
| alpha | L1 regularization term on weights\. Increasing this value makes models more conservative\. **Optional** Valid values: float Default value: 0  | 
| base\_score | The initial prediction score of all instances, global bias\. **Optional** Valid values: float Default value: 0\.5  | 
| booster | Which booster to use\. The `gbtree` and `dart` values use a tree\-based model, while `gblinear` uses a linear function\. **Optional** Valid values: String\. One of `gbtree`, `gblinear`, or `dart`\. Default value: `gbtree`  | 
| colsample\_bylevel | Subsample ratio of columns for each split, in each level\. **Optional** Valid values: Float\. Range: \[0,1\]\. Default value: 1  | 
| colsample\_bytree | Subsample ratio of columns when constructing each tree\. **Optional** Valid values: Float\. Range: \[0,1\]\. Default value: 1 | 
| csv\_weights | When this flag is enabled, XGBoost differentiates the importance of instances for csv input by taking the second column \(the column after labels\) in training data as the instance weights\. **Optional** Valid values: 0 or 1 Default value: 0  | 
| early\_stopping\_rounds | The model trains until the validation score stops improving\. Validation error needs to decrease at least every `early_stopping_rounds` to continue training\. SageMaker hosting uses the best model for inference\. **Optional** Valid values: integer Default value: \-  | 
| eta | Step size shrinkage used in updates to prevent overfitting\. After each boosting step, you can directly get the weights of new features\. The `eta` parameter actually shrinks the feature weights to make the boosting process more conservative\. **Optional** Valid values: Float\. Range: \[0,1\]\. Default value: 0\.3  | 
| eval\_metric | Evaluation metrics for validation data\. A default metric is assigned according to the objective:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/xgboost-72.html) For a list of valid inputs, see [XGBoost Parameters](https://github.com/dmlc/xgboost/blob/master/doc/parameter.rst#learning-task-parameters)\. **Optional** Valid values: string Default value: Default according to objective\.  | 
| gamma | Minimum loss reduction required to make a further partition on a leaf node of the tree\. The larger, the more conservative the algorithm is\. **Optional** Valid values: Float\. Range: \[0,∞\)\. Default value: 0  | 
| grow\_policy | Controls the way that new nodes are added to the tree\. Currently supported only if `tree_method` is set to `hist`\. **Optional** Valid values: String\. Either `depthwise` or `lossguide`\. Default value: `depthwise`  | 
| lambda | L2 regularization term on weights\. Increasing this value makes models more conservative\. **Optional** Valid values: float Default value: 1  | 
| lambda\_bias | L2 regularization term on bias\. **Optional** Valid values: Float\. Range: \[0\.0, 1\.0\]\. Default value: 0  | 
| max\_bin | Maximum number of discrete bins to bucket continuous features\. Used only if `tree_method` is set to `hist`\.  **Optional** Valid values: integer Default value: 256  | 
| max\_delta\_step | Maximum delta step allowed for each tree's weight estimation\. When a positive integer is used, it helps make the update more conservative\. The preferred option is to use it in logistic regression\. Set it to 1\-10 to help control the update\.  **Optional** Valid values: Integer\. Range: \[0,∞\)\. Default value: 0  | 
| max\_depth | Maximum depth of a tree\. Increasing this value makes the model more complex and likely to be overfit\. 0 indicates no limit\. A limit is required when `grow_policy`=`depth-wise`\. **Optional** Valid values: Integer\. Range: \[0,∞\) Default value: 6  | 
| max\_leaves | Maximum number of nodes to be added\. Relevant only if `grow_policy` is set to `lossguide`\. **Optional** Valid values: integer Default value: 0  | 
| min\_child\_weight | Minimum sum of instance weight \(hessian\) needed in a child\. If the tree partition step results in a leaf node with the sum of instance weight less than `min_child_weight`, the building process gives up further partitioning\. In linear regression models, this simply corresponds to a minimum number of instances needed in each node\. The larger the algorithm, the more conservative it is\. **Optional** Valid values: Float\. Range: \[0,∞\)\. Default value: 1  | 
| normalize\_type | Type of normalization algorithm\. **Optional** Valid values: Either *tree* or *forest*\. Default value: *tree*  | 
| nthread | Number of parallel threads used to run *xgboost*\. **Optional** Valid values: integer Default value: Maximum number of threads\.  | 
| objective | Specifies the learning task and the corresponding learning objective\. Examples: `reg:logistic`, `reg:softmax`, `multi:squarederror`\. For a full list of valid inputs, refer to [XGBoost Parameters](https://github.com/dmlc/xgboost/blob/master/doc/parameter.rst#learning-task-parameters)\. **Optional** Valid values: string Default value: `reg:squarederror`  | 
| one\_drop | When this flag is enabled, at least one tree is always dropped during the dropout\. **Optional** Valid values: 0 or 1 Default value: 0  | 
| process\_type | The type of boosting process to run\. **Optional** Valid values: String\. Either `default` or `update`\. Default value: `default`  | 
| rate\_drop | The dropout rate that specifies the fraction of previous trees to drop during the dropout\. **Optional** Valid values: Float\. Range: \[0\.0, 1\.0\]\. Default value: 0\.0  | 
| refresh\_leaf | This is a parameter of the 'refresh' updater plug\-in\. When set to `true` \(1\), tree leaves and tree node stats are updated\. When set to `false`\(0\), only tree node stats are updated\. **Optional** Valid values: 0/1 Default value: 1  | 
| sample\_type | Type of sampling algorithm\. **Optional** Valid values: Either `uniform` or `weighted`\. Default value: `uniform`  | 
| scale\_pos\_weight | Controls the balance of positive and negative weights\. It's useful for unbalanced classes\. A typical value to consider: `sum(negative cases)` / `sum(positive cases)`\. **Optional** Valid values: float Default value: 1  | 
| seed | Random number seed\. **Optional** Valid values: integer Default value: 0  | 
| silent | 0 means print running messages, 1 means silent mode\. Valid values: 0 or 1 **Optional** Default value: 0  | 
| sketch\_eps | Used only for approximate greedy algorithm\. This translates into O\(1 / `sketch_eps`\) number of bins\. Compared to directly select number of bins, this comes with theoretical guarantee with sketch accuracy\. **Optional** Valid values: Float, Range: \[0, 1\]\. Default value: 0\.03  | 
| skip\_drop | Probability of skipping the dropout procedure during a boosting iteration\. **Optional** Valid values: Float\. Range: \[0\.0, 1\.0\]\. Default value: 0\.0  | 
| subsample | Subsample ratio of the training instance\. Setting it to 0\.5 means that XGBoost randomly collects half of the data instances to grow trees\. This prevents overfitting\. **Optional** Valid values: Float\. Range: \[0,1\]\. Default value: 1  | 
| tree\_method | The tree construction algorithm used in XGBoost\. **Optional** Valid values: One of `auto`, `exact`, `approx`, or `hist`\. Default value: `auto`  | 
| tweedie\_variance\_power | Parameter that controls the variance of the Tweedie distribution\. **Optional** Valid values: Float\. Range: \(1, 2\)\. Default value: 1\.5  | 
| updater | A comma\-separated string that defines the sequence of tree updaters to run\. This provides a modular way to construct and to modify the trees\. For a full list of valid inputs, please refer to [XGBoost Parameters](https://github.com/dmlc/xgboost/blob/master/doc/parameter.rst)\. **Optional** Valid values: comma\-separated string\. Default value: `grow_colmaker`, prune  | 

## Tune an XGBoost Release 0\.72 Model<a name="xgboost-72-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your training and validation datasets\. You choose three types of hyperparameters:
+ a learning `objective` function to optimize during model training
+ an `eval_metric` to use to evaluate model perrormance during validation
+ a set of hyperparameters and a range of values for each to use when tuning the model automatically

You choose the evaluation metric from set of evaluation metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the evaluation metric\. 

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

### Metrics Computed by the XGBoost Release 0\.72 Algorithm<a name="xgboost-72-metrics"></a>

The XGBoost algorithm based on version 0\.72 computes the following nine metrics to use for model validation\. When tuning the model, choose one of these metrics to evaluate the model\. For full list of valid `eval_metric` values, refer to [XGBoost Learning Task Parameters](https://github.com/dmlc/xgboost/blob/master/doc/parameter.rst#learning-task-parameters)


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| validation:auc |  Area under the curve\.  |  Maximize  | 
| validation:error |  Binary classification error rate, calculated as \#\(wrong cases\)/\#\(all cases\)\.  |  Minimize  | 
| validation:logloss |  Negative log\-likelihood\.  |  Minimize  | 
| validation:mae |  Mean absolute error\.  |  Minimize  | 
| validation:map |  Mean average precision\.  |  Maximize  | 
| validation:merror |  Multiclass classification error rate, calculated as \#\(wrong cases\)/\#\(all cases\)\.  |  Minimize  | 
| validation:mlogloss |  Negative log\-likelihood for multiclass classification\.  |  Minimize  | 
| validation:ndcg |  Normalized Discounted Cumulative Gain\.  |  Maximize  | 
| validation:rmse |  Root mean square error\.  |  Minimize  | 

### Tunable XGBoost Release 0\.72 Hyperparameters<a name="xgboost-72-tunable-hyperparameters"></a>

Tune the XGBoost model with the following hyperparameters\. The hyperparameters that have the greatest effect on optimizing the XGBoost evaluation metrics are: `alpha`, `min_child_weight`, `subsample`, `eta`, and `num_round`\. 


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| alpha |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 1000  | 
| colsample\_bylevel |  ContinuousParameterRanges  |  MinValue: 0\.1, MaxValue: 1  | 
| colsample\_bytree |  ContinuousParameterRanges  |  MinValue: 0\.5, MaxValue: 1  | 
| eta |  ContinuousParameterRanges  |  MinValue: 0\.1, MaxValue: 0\.5  | 
| gamma |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 5  | 
| lambda |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 1000  | 
| max\_delta\_step |  IntegerParameterRanges  |  \[0, 10\]  | 
| max\_depth |  IntegerParameterRanges  |  \[0, 10\]  | 
| min\_child\_weight |  ContinuousParameterRanges  |  MinValue: 0, MaxValue: 120  | 
| num\_round |  IntegerParameterRanges  |  \[1, 4000\]  | 
| subsample |  ContinuousParameterRanges  |  MinValue: 0\.5, MaxValue: 1  | 