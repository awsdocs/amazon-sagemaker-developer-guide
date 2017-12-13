# Linear Learner Hyperparameters<a name="ll_hyperparameters"></a>


| Parameter Name | Description | 
| --- | --- | 
| feature\_dim |  Number of features in input data\. Required\. Valid values: positive integer Default value: \-  | 
| predictor\_type |  Whether the target variable is binary classification or regression\. Required\. Valid values: `binary_classifier` or *regressor* Default value: \-  | 
| mini\_batch\_size |  Mini batch size for data iterator, consisting of number of observations per mini batch\. Valid values: positive integer Default value: 1000  | 
| epochs |  Maximum number of passes over the training data\. Valid values: positive integer Default value: 10  | 
| use\_bias |  Whether the model should include bias \(also called an intercept\) feature\. Valid values: *true* or *false* Default value: *true*  | 
| binary\_classifier\_model\_selection\_criteria |  Pick the model with best criteria from the validation dataset for `predictor_type` `binary_classifier`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/ll_hyperparameters.html) Valid values: *accuracy*, *f1*, `precision_at_target_recall`, `recall_at_target_precision`, `cross_entropy_loss` Default value: *accuracy*  | 
| target\_recall |  Target recall\. Applicable only if `binary_classifier_model_selection_criteria` is `precision_at_target_recall`\. Valid values: Number between 0 and 1\.0 Default value: 0\.8  | 
| target\_precision |  Target precision\. Only applicable if `binary_classifier_model_selection_criteria` is `recall_at_target_precision`\. Valid values: Number between 0 and 1\.0 Default value: 0\.8  | 
| num\_models |  Description: Number of models to train in parallel\. For the default *auto*, the algorithm decides the number of parallel models to train\. One model is trained according to the given training parameter \(regularization, optimizer, loss\), and the rest by close parameters\. Valid values: positive integer or *auto* Default values: *auto*  | 
| num\_calibration\_samples |  Number of observations to use from the validation dataset for model calibration \(finding the best threshold\)\. Valid values: Positive integer or *auto* Default value: *auto*  | 
| init\_method |  Function to use to set the initial model weights\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/ll_hyperparameters.html) Valid values: *uniform* or *normal* Default value: *uniform*  | 
| init\_scale | Scale\. Applies only when init\_method is set to uniform\. Valid values: numbers \-1\.0 and 1\.0Default value: 0\.07 | 
| init\_sigma | Standard deviation\. Applies only when init\_method is set to normal\. Optional\.Valid values: number between 0 and 1Default value: 0\.01 | 
| init\_bias |  Initial weight for bias term\. Valid values: number Default value: 0  | 
| optimizer | The optimizer to use\. Default setting for auto is adam\. Valid values: *sgd*, *adam*, or *auto*\. Default value: *auto* | 
| loss |  The loss function to apply\. The default *auto* is *logistic* for `predictor_type` *binary\_classifier* and *squared\_loss* for `predictor_type` *regressor*\. Valid values: *logistic*, *squared\_loss*, or *absolute\_loss* Default value: *auto*  | 
| wd |  L2 regularization parameter\. In other words, the weight decay parameter\. Use 0 for no L2 regularization\. Valid values: number between 0 and 1\.0, *auto* Default value: *auto*  | 
| l1 |  L1 regularization parameter\. Use 0 for no L1 regularization\.  Valid values: number between 0 and 1\.0, *auto* Default value: *auto*  | 
| momentum | Momentum parameter of the sgd optimizer\. Valid values: number between 0 and 1\.0Default value: 0 | 
| learning\_rate | The default, auto, depends on the optimizer chosen\. Valid values: number between 0 and 1\.0, *auto* Default value: *auto* | 
| beta\_1 | Exponential decay rate for first moment estimates\. Applies only when adam optimizer\.Valid values: number between 0 and 1\.0Default value: 0\.9 | 
| beta\_2 | Exponential decay rate for second moment estimates\. Only applies for adam optimizer\. Optional\.Valid values: Number between 0 and 1\.0Default value: 0\.999 | 
| bias\_lr\_mult | Allows a different learning rate for the bias term\. The actual learning rate for the bias is learning rate times bias\_lr\_mult\. Optional\.Valid values: positive floatDefault value: 10 | 
| bias\_wd\_mult | Allows different regularization for the bias term\. The actual L2 regularization weight for the bias is wd times bias\_wd\_mult\. By default there is no regularization on the bias term\. Optional\.Valid values: positive floatDefault value: 0 | 
| use\_lr\_scheduler |  If true, uses a scheduler for the learning rate\. Valid values: \(*true* or *false*\)\. Default value: *true*  | 
| lr\_scheduler\_step |  The number of steps between decreases of the learning rate\. Only applies to learning rate scheduler\. Valid values: positive integer Default value: 100  | 
| lr\_scheduler\_factor | For every lr\_scheduler\_step, the learning rate decreases by this quantity\. Applies only for learning rate scheduler\. OptionalValid values: positive float between 0 and 1Default value: 0\.99 | 
| lr\_scheduler\_minimum\_lr | The learning rate never decreases to a value lower than lr\_scheduler\_minimum\_lr\. Applies only for learning rate scheduler\.Valid values: positive floatDefault values: 0\.00001 | 
| normalize\_data | Normalizes the features before training to have `std_dev` of 1\. Optional\. Valid values: *true*, *false*, or *auto* Default value: *true* | 
| normalize\_label | Normalizes label\. For regression, the label is normalized, for classification, it is not\. If this is set to *true* during classification, this parameter is ignored\. Valid values: *true*, *false*, or *auto* Default value: *auto* | 
| unbias\_data | Unbiases the features before training so the mean is 0\. By default data is unbiased if `use_bias` is set to *true*\. Valid values: *true*, *false*, or *auto* Default value: *auto* | 
| unbias\_label | Unbiases labels before training so the mean is 0\. Only done for regrssion if `use_bias` is set to *true*\. Valid values: *true*, *false*, or *auto* Default value: *auto* | 
| num\_point\_for\_scaler | Number of data points to use for calcuating normalization or unbiasing of terms\. Valid values: positive integer Default value: 10,000 | 