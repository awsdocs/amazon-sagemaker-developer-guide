# Factorization Machines Hyperparameters<a name="fact-machines-hyperparameters"></a>

The following table contains the hyperparameters for the factorization machines algorithm\. These are parameters that are set by users to facilitate the estimation of model parameters from data\. The required hyperparameters that must be set are listed first, in alphabetical order\. The optional hyperparameters that can be set are listed next, also in alphabetical order\.


| Parameter Name | Description | 
| --- | --- | 
| feature\_dim | The dimension of the input feature space\. This could be very high with sparse input\. **Required** Valid values: Positive integer\. Suggested value range: \[10000,10000000\]  | 
| num\_factors | The dimensionality of factorization\. **Required** Valid values: Positive integer\. Suggested value range: \[2,1000\], 64 is usually optimal\.  | 
| predictor\_type | The type of predictor\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/fact-machines-hyperparameters.html) **Required** Valid values: String: `binary_classifier` or `regressor`  | 
| bias\_init\_method | The initialization method for the bias term: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/fact-machines-hyperparameters.html) **Optional** Valid values: `uniform`, `normal`, or `constant` Default value: `normal`  | 
| bias\_init\_scale | Range for initialization of the bias term\. Takes effect if `bias_init_method` is set to `uniform`\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: None  | 
| bias\_init\_sigma | The standard deviation for initialization of the bias term\. Takes effect if `bias_init_method` is set to `normal`\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.01  | 
| bias\_init\_value | The initial value of the bias term\. Takes effect if `bias_init_method` is set to `constant`\.  **Optional** Valid values: Float\. Suggested value range: \[1e\-8, 512\]\. Default value: None  | 
| bias\_lr | The learning rate for the bias term\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.1  | 
| bias\_wd | The weight decay for the bias term\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.01  | 
| clip\_gradient | Gradient clipping optimizer parameter\. Clips the gradient by projecting onto the interval \[\-`clip_gradient`, \+`clip_gradient`\]\.  **Optional** Valid values: Float Default value: None  | 
| epochs | The number of training epochs to run\.  **Optional** Valid values: Positive integer Default value: 1  | 
| eps | Epsilon parameter to avoid division by 0\. **Optional** Valid values: Float\. Suggested value: small\. Default value: None  | 
| factors\_init\_method | The initialization method for factorization terms: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/fact-machines-hyperparameters.html) **Optional** Valid values: `uniform`, `normal`, or `constant`\. Default value: `normal`  | 
| factors\_init\_scale  | The range for initialization of factorization terms\. Takes effect if `factors_init_method` is set to `uniform`\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: None  | 
| factors\_init\_sigma | The standard deviation for initialization of factorization terms\. Takes effect if `factors_init_method` is set to `normal`\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.001  | 
| factors\_init\_value | The initial value of factorization terms\. Takes effect if `factors_init_method` is set to `constant`\.  **Optional** Valid values: Float\. Suggested value range: \[1e\-8, 512\]\. Default value: None  | 
| factors\_lr | The learning rate for factorization terms\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.0001  | 
| factors\_wd | The weight decay for factorization terms\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.00001  | 
| linear\_lr | The learning rate for linear terms\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.001  | 
| linear\_init\_method | The initialization method for linear terms: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/fact-machines-hyperparameters.html) **Optional** Valid values: `uniform`, `normal`, or `constant`\. Default value: `normal`  | 
| linear\_init\_scale | Range for initialization of linear terms\. Takes effect if `linear_init_method` is set to `uniform`\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: None  | 
| linear\_init\_sigma | The standard deviation for initialization of linear terms\. Takes effect if `linear_init_method` is set to `normal`\.  **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.01  | 
| linear\_init\_value | The initial value of linear terms\. Takes effect if `linear_init_method` is set to *constant*\.  **Optional** Valid values: Float\. Suggested value range: \[1e\-8, 512\]\. Default value: None  | 
| linear\_wd | The weight decay for linear terms\. **Optional** Valid values: Non\-negative float\. Suggested value range: \[1e\-8, 512\]\. Default value: 0\.001  | 
| mini\_batch\_size | The size of mini\-batch used for training\.  **Optional** Valid values: Positive integer Default value: 1000  | 
| rescale\_grad |  Gradient rescaling optimizer parameter\. If set, multiplies the gradient with `rescale_grad` before updating\. Often choose to be 1\.0/`batch_size`\.  **Optional** Valid values: Float Default value: None  | 