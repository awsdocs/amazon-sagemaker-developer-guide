# XGBoost Hyperparameters<a name="xgboost_hyperparameters"></a>

The Amazon SageMaker XGBoost algorithm is an implementation of the open\-source XGBoost package\. For more detail about hyperparameter configurations, see [here](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md)\. 


| Parameter Name | Description | 
| --- | --- | 
| num\_round | The number of rounds to run the training\. Required\. Valid values: integer Default value: \-  | 
| num\_class | Required if `objective` is set to *multi:softmax* or *multi:softprob*\. Valid values: integer Default value: \-  | 
| booster | Which booster to use\. The *gbtree* and *dart* values use a tree\-based model, while *gblinear* uses a linear function\. Valid values: String\. One of *gbtree*, *gblinear*, or *dart*\. Default value: *gbtree*  | 
| silent | 0 means print running messages, 1 means silent mode\. Valid values: 0 or 1 Default value: 0  | 
| nthread | Number of parallel threads used to run *xgboost*\. Valid values: integer Default value: Maximum number of threads\.  | 
| eta | Step size shrinkage used in updates to prevent overfitting\. After each boosting step, you can directly get the weights of new features\. The `eta` parameter actually shrinks the feature weights to make the boosting process more conservative\. Valid values: Float\. Range: \[0,1\]\. Default value: 0\.3  | 
| gamma | Minimum loss reduction required to make a further partition on a leaf node of the tree\. The larger, the more conservative the algorithm is\. Valid values: Float\. Range: \[0,∞\)\. Default value: 0  | 
| max\_depth | Maximum depth of a tree\. Increasing this value makes the model more complex and likely to be overfitted\. 0 indicates no limit\. A limit is required when `grow_policy`=`depth-wise`\. Valid values: Integer\. Range: \[0,∞\) Default value: 6  | 
| min\_child\_weight | Minimum sum of instance weight \(hessian\) needed in a child\. If the tree partition step results in a leaf node with the sum of instance weight less than `min_child_weight`, the building process gives up further partitioning\. In linear regression models, this simply corresponds to a minimum number of instances needed in each node\. The larger the algorithm, the more conservative it is\. Valid values: Float\. Range: \[0,∞\)\. Default value: 1  | 
| max\_delta\_step | Maximum delta step allowed for each tree's weight estimation\. Valid inputs: When a positive integer is used, it helps make the update more conservative\. The preferred option is to use it in logistic regression\. Set it to 1\-10 to help control the update\.  Valid values: Integer\. Range: \[0,∞\)\. Default value: 0  | 
| subsample | Subsample ratio of the training instance\. Setting it to 0\.5 means that *XGBoost* randomly collects half of the data instances to grow trees\. This prevents overfitting\. Valid values: Float\. Range: \[0,1\]\. Default value: 1  | 
| colsample\_bytree | Subsample ratio of columns when constructing each tree\. Valid values: Float\. Range: \[0,1\]\. Default value: 1 | 
| colsample\_bylevel | Subsample ratio of columns for each split, in each level\. Valid values: Float\. Range: \[0,1\]\. Default value: 1  | 
| lambda | L2 regularization term on weights\. Increasing this value makes models more conservative\. Valid values: float Default value: 1  | 
| alpha | L1 regularization term on weights\. Increasing this value makes models more conservative\. Valid values: float Default value: 1  | 
| tree\_method | The tree construction algorithm used in *XGBoost*\. Valid values: One of *auto*, *exact*, *approx*, or *hist*\. Default value: *auto*  | 
| sketch\_eps | Used only for approximate greedy algorithm\. This translates into O\(1 / `sketch_eps`\) number of bins\. Compared to directly select number of bins, this comes with theoretical guarantee with sketch accuracy\. Valid values: Float, Range: \[0, 1\]\. Default value: 0\.03  | 
| scale\_pos\_weight | Controls the balance of positive and negative weights\. It's useful for unbalanced classes\. A typical value to consider: `sum(negative cases)` / `sum(positive cases)`\. Valid values: float Default value: 1  | 
| updater | A comma\-separated string that defines the sequence of tree updaters to run\. This provides a modular way to construct and to modify the trees\. For a full list of valid inputs, please refer to [XGBoost Parameters](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md)\. Valid values: comma\-separated string\. Default value: `grow_colmaker`, prune  | 
| refresh\_leaf | This is a parameter of the 'refresh' updater plugin\. When set to `true`, tree leaves and tree node stats are updated\. When set to `false`, only tree node stats are updated\. Valid values: 0/1 Default value: 1  | 
| process\_type | The type of boosting process to run\. Valid values: String\. Either *default* or *update*\. Default value: *default*  | 
| grow\_policy | Controls the way that new nodes are added to the tree\. Currently supported only if `tree_method` is set to *hist*\. Valid values: String\. Either *depthwise* or *lossguide*\. Default value: *depthwise*  | 
| max\_leaves | Maximum number of nodes to be added\. Relevant only if `grow_policy` is set to *lossguide*\. Valid values: integer Default value: 0  | 
| max\_bin | Maximum number of discrete bins to bucket continuous features\. Used only if `tree_method` is set to *hist*\.  Valid values: integer Default value: 256  | 
| sample\_type | Type of sampling algorithm\. Valid values: Either *uniform* or *weighted*\. Default value: *uniform*  | 
| normalize\_type | Type of normalization algorithm\. Valid values: Either *tree* or *forest*\. Default value: *tree*  | 
| rate\_drop | Dropout rate \(a fraction of previous trees to drop during the dropout\)\. Valid values: Float\. Range: \[0\.0, 1\.0\]\. Default value: 0\.0  | 
| one\_drop | When this flag is enabled, at least one tree is always dropped during the dropout\. Valid values: 0 or 1 Default value: 0  | 
| skip\_drop | Probability of skipping the dropout procedure during a boosting iteration\. Valid values: Float\. Range: \[0\.0, 1\.0\]\. Default value: 0\.0  | 
| lambda\_bias | L2 regularization term on bias\. Valid values: Float\. Range: \[0\.0, 1\.0\]\. Default value: 0  | 
| tweedie\_variance\_power | Parameter that controls the variance of the Tweedie distribution\. Valid values: Float\. Range: \[1, 2\]\. Default value: 1\.5  | 
| objective | Specifies the learning task and the corresponding learning objective\. Examples: *reg:linear*, *reg:logistic*, *multi:softmax*\. For a full list of valid inputs, please refer to [XGBoost Parameters](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md)\. Valid values: string Default value: *reg:linear*  | 
| base\_score | The initial prediction score of all instances, global bias\. Valid values: float Default value: 0\.5  | 
| eval\_metric | Evaluation metrics for validation data\. A default metric is assigned according to the objective \(*rmse* for regression, *error* for classification, and *map* for ranking\)\. For a list of valid inputs, see [XGBoost Parameters](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md)\. Valid values: string Default value: Default according to objective\.  | 
| seed | Random number seed\. Valid values: integer Default value: 0  | 
| early\_stopping\_rounds | The model will train until the validation score stops improving\. Validation error needs to decrease at least every `early_stopping_rounds` to continue training\. Amazon SageMaker hosting will use the best model for inference\. Valid values: integer Default value: \-  | 