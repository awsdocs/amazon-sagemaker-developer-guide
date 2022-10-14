# TabTransformer hyperparameters<a name="tabtransformer-hyperparameters"></a>

The following table contains the subset of hyperparameters that are required or most commonly used for the Amazon SageMaker TabTransformer algorithm\. Users set these parameters to facilitate the estimation of model parameters from data\. The SageMaker TabTransformer algorithm is an implementation of the open\-source [TabTransformer](https://github.com/jrzaurin/pytorch-widedeep) package\.

**Note**  
The default hyperparameters are based on example datasets in the [TabTransformer sample notebooks](tabtransformer.md#tabtransformer-sample-notebooks)\.

The SageMaker TabTransformer algorithm automatically chooses an evaluation metric and objective function based on the type of classification problem\. The TabTransformer algorithm detects the type of classification problem based on the number of labels in your data\. For regression problems, the evaluation metric is r square and the objective function is mean square error\. For binary classification problems, the evaluation metric and objective function are both binary cross entropy\. For multiclass classification problems, the evaluation metric and objective function are both multiclass cross entropy\.

**Note**  
The TabTransformer evaluation metric and objective functions are not currently available as hyperparameters\. Instead, the SageMaker TabTransformer built\-in algorithm automatically detects the type of classification task \(regression, binary, or multiclass\) based on the number of unique integers in the label column and assigns an evaluation metric and objective function\.


| Parameter Name | Description | 
| --- | --- | 
| n\_epochs |  Number of epochs to train the deep neural network\. Valid values: integer, range: Positive integer\. Default value: `5`\.  | 
| patience |  The training will stop if one metric of one validation data point does not improve in the last `patience` round\. Valid values: integer, range: \(`2`, `60`\)\. Default value: `10`\.  | 
| learning\_rate |  The rate at which the model weights are updated after working through each batch of training examples\. Valid values: float, range: Positive floating point number\. Default value: `0.001`\.  | 
| batch\_size |  The number of examples propagated through the network\.  Valid values: integer, range: \(`1`, `2048`\)\. Default value: `256`\.  | 
| input\_dim |  The dimension of embeddings to encode the categorical and/or continuous columns\. Valid values: string, any of the following: `"16"`, `"32"`, `"64"`, `"128"`, `"256"`, or `"512"`\. Default value: `"32"`\.  | 
| n\_blocks |  The number of Transformer encoder blocks\. Valid values: integer, range: \(`1`, `12`\)\. Default value: `4`\.  | 
| attn\_dropout |  Dropout rate applied to the Multi\-Head Attention layers\. Valid values: float, range: \(`0`, `1`\)\. Default value: `0.2`\.  | 
| mlp\_dropout |  Dropout rate applied to the FeedForward network within the encoder layers and the final MLP layers on top of Transformer encoders\. Valid values: float, range: \(`0`, `1`\)\. Default value: `0.1`\.  | 
| frac\_shared\_embed |  The fraction of embeddings shared by all the different categories for one particular column\. Valid values: float, range: \(`0`, `1`\)\. Default value: `0.25`\.  | 