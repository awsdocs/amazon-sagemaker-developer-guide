# Factorization Machines Hyperparameters<a name="fact-machines-hyperparameters"></a>


| Parameter Name | Description | 
| --- | --- | 
| feature\_dim | Dimension of the input feature space\. This could be very high with sparse input\. Required\. Valid values: Positive integer\. Suggested value range: \[10000,10000000\] Default value: \-  | 
| mini\_batch\_size | Size of mini\-batch used for training\.  Valid values: positive integer Default value: 1000  | 
| epochs | Number of training epochs to run\.  Valid values: positive integer Default value: 1  | 
| num\_factors | Dimensionality of factorization\. Required\. Valid values: Positive integer\. Suggested value range: \[2,1000\] Default value: \-  | 
| predictor\_type | Type of predictor\. Required\. Valid values: String: `binary_classifier` or regressor Default value: \-  | 
| clip\_gradient | Optimizer parameter\. Clip the gradient by projecting onto the box \[\-`clip_gradient`, \+`clip_gradient`\]\.  Valid values: float Default value: \-  | 
| eps | Optimizer parameter\. Small value to avoid division by 0\.  Valid values: float Default value: \-  | 
| rescale\_grad | Optimizer parameter\. If set, multiplies the gradient with `rescale_grad` before updating\. Often choose to be 1\.0/`batch_size`\.  Valid values: float Default value: \-  | 
| bias\_lr | Learning rate for the bias term\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.1  | 
| linear\_lr | Learning rate for linear terms\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.001  | 
| factors\_lr | Learning rate for factorization terms\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.0001  | 
| bias\_wd | Weight decay for the bias term\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.01  | 
| linear\_wd | Weight decay for linear terms\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.001  | 
| factors\_wd | Weight decay for factorization terms\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.00001  | 
| bias\_init\_method | Initialization method for the bias term\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/fact-machines-hyperparameters.html) Valid values: One of *uniform*, *normal*, or *constant* Default value: *normal*  | 
| bias\_init\_scale | Range for initialization of the bias term\. Takes effect if `bias_init_method` is set to *uniform*\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: \-  | 
| bias\_init\_sigma | Standard deviation for initialization of the bias term\. Takes effect if `bias_init_method` is set to *normal*\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.01  | 
| bias\_init\_value | Initial value of the bias term\. Takes effect if `bias_init_method` is set to *constant*\.  Valid values: Float\. Suggested value range: \[1e\-8, 512\]/ Default value: \-  | 
| linear\_init\_method | Initialization method for linear terms\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/fact-machines-hyperparameters.html) Valid values: One of *uniform*, *normal*, or *constant*\. Default value: *normal*  | 
| linear\_init\_scale | Range for initialization of linear terms\. Takes effect if `linear_init_method` is set to *uniform*\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: \-  | 
| linear\_init\_sigma | Standard deviation for initialization of linear terms\. Takes effect if `linear_init_method` is set to *normal*\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.01  | 
| linear\_init\_value | Initial value of linear terms\. Takes effect if `linear_init_method` is set to *constant*\.  Valid values: Float\. Suggested value range: \[1e\-8, 512\]\. Default value: \-  | 
| factors\_init\_method | Initialization method for factorization terms\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/fact-machines-hyperparameters.html) Valid values: One of *uniform*, *normal*, or *constant*\. Default value: *normal*  | 
| factors\_init\_scale  | Range for initialization of factorization terms\. Takes effect if `factors_init_method` is set to *uniform*\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: \-  | 
| factors\_init\_sigma | Standard deviation for initialization of factorization terms\. Takes effect if `factors_init_method` is set to *normal*\.  Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.001  | 
| factors\_init\_value | Initial value of factorization terms\. Takes effect if `factors_init_method` is set to *constant*\.  Valid values: Float\. Suggested value range: \[1e\-8, 512\]\. Default value: \-  | 