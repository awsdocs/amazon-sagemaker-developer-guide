# Object2Vec Hyperparameters<a name="object2vec-hyperparameters"></a>


| Parameter Name | Description | 
| --- | --- | 
| enc0\_max\_seq\_len | The maximum sequence length for the enc0 encoder\. **Required** Valid values: 1 ≤ integer ≤ 5000  | 
| enc0\_vocab\_size | The vocabulary size of enc0 tokens\. **Required** Valid values: 2 ≤ integer ≤ 3000000  | 
| bucket\_width | The allowed difference between data sequence length when bucketing is enabled\. Bucketing is enabled when a non\-zero value is specified for this parameter\. **Optional** Valid values: 0 ≤ integer ≤ 100 Default value: 0 \(no bucketing\)  | 
| dropout | The dropout probability for network layers\. Dropout is a form of regularization used in neural networks that reduces overfitting by trimming codependent neurons\. **Optional** Valid values: 0\.0 ≤ float ≤ 1\.0 Default value: 0\.0  | 
| early\_stopping\_patience | The number of consecutive epochs without improvement allowed before early stopping is applied\. Improvement is defined by the `early_stopping_tolerance`\. **Optional** Valid values: 1 ≤ integer ≤ 5 Default value: 3  | 
| early\_stopping\_tolerance | The reduction in the loss function that an algorithm must achieve between consecutive epochs to avoid early stopping after an `early_stopping_patience` number of consecutive epochs\. **Optional** Valid values: 0\.000001 ≤ float ≤ 0\.1 Default value: 0\.01  | 
| enc\_dim | The dimension of the output of the embedding layer\. **Optional** Valid values: 4 ≤ integer ≤ 10000 Default value: 4096  | 
| enc0\_network | Network model for the enc0 encoder\. **Optional** Valid values: `hcnn`, `bilstm`, or `pooled_embedding` [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/object2vec-hyperparameters.html) Default value: `hcnn`  | 
| enc0\_cnn\_filter\_width | The filter width of the convolutional neural network \(CNN\) enc0 encoder\. **Conditional** Valid values: 1 ≤ integer ≤ 9 Default value: 3  | 
| enc0\_freeze\_pretrained\_embedding | Whether to freeze enc0 pretrained embedding weights\. **Conditional** Valid values: `True` or `False` Default value: `True`  | 
| enc0\_layers  | The number of layers in the enc0 encoder\. **Conditional** Valid values: `auto` or 1 ≤ integer ≤ 4 Default value: `auto`  | 
| enc0\_pretrained\_embedding\_file | The filename of pretrained enc0 token embedding file in the auxiliary data channel\. **Conditional** Valid values: String with alphanumeric characters, underscore, or period\. \[A\-Za\-z0\-9\\\.\\\_\]  Default value: "" \(empty string\)  | 
| enc0\_token\_embedding\_dim | The output dimension of the enc0 token embedding layer\. **Conditional** Valid values: 2 ≤ integer ≤ 1000 Default value: 300  | 
| enc0\_vocab\_file | The vocabulary file for mapping pretrained enc0 token embeddings to vocabulary IDs\. **Conditional** Valid values: String with alphanumeric characters, underscore, or period\. \[A\-Za\-z0\-9\\\.\\\_\]  Default value: "" \(empty string\)  | 
| enc1\_network | The network model for the enc1 encoder\. If its value is set to `enc0`, then enc1 uses the same network model as enc0, including the hyperparameter values\. Note that although the enc0 and enc1 encoder networks may have symmetric architecture, shared parameter values for these networks is not supported\. **Optional** Valid values: `hcnn`, `bilstm`, or `pooled_embedding` [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/object2vec-hyperparameters.html) Default value: `enc0`  | 
| enc1\_cnn\_filter\_width |  The filter width of the CNN enc1 encoder\. **Conditional** Valid values: 1 ≤ integer ≤ 9 Default value: 3  | 
| enc1\_freeze\_pretrained\_embedding | Whether to freeze enc1 pretrained embedding weights\. **Conditional** Valid values: `True` or `False` Default value: `True`  | 
| enc1\_layers  | The number of layers in the enc1 encoder\. **Conditional** Valid values: `auto` or 1 ≤ integer ≤ 4 Default value: `auto`  | 
| enc1\_max\_seq\_len | The maximum sequence length for the enc1 encoder\. **Conditional** Valid values: 1 ≤ integer ≤ 5000  | 
| enc1\_pretrained\_embedding\_file | The filename of the enc1 pretrained token embedding file in the auxiliary data channel\. **Conditional** Valid values: String with alphanumeric characters, underscore, or period\. \[A\-Za\-z0\-9\\\.\\\_\]  Default value: "" \(empty string\)  | 
| enc1\_token\_embedding\_dim | The output dimension of enc1 token embedding layer\. **Conditional** Valid values: 2 ≤ integer ≤ 1000 Default value: 300  | 
| enc1\_vocab\_file | The vocabulary file for mapping pretrained enc1 token embeddings to vocabulary IDs **Conditional** Valid values: String with alphanumeric characters, underscore, or period\. \[A\-Za\-z0\-9\\\.\\\_\]  Default value: "" \(empty string\)  | 
| enc1\_vocab\_size | The vocabulary size of enc0 tokens\. **Conditional** Valid values: 2 ≤ integer ≤ 3000000  | 
| epochs | The number of epochs to run for training\.  **Optional** Valid values: 1 ≤ integer ≤ 100 Default value: 30  | 
| learning\_rate | The learning rate for training\. **Optional** Valid values: 1\.0e\-6 ≤ float ≤ 1\.0 Default value: 0\.0004  | 
| mini\_batch\_size | The batch size that the data set is split into for an `optimizer` during training\. **Optional** Valid values: 1 ≤ integer ≤ 10000 Default value: 32  | 
| mlp\_activation | The type of activation function for the multilayer perceptron \(MLP\) layer\. **Optional** Valid values: `tanh`, `relu`, or `linear` [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/object2vec-hyperparameters.html) Default value: `linear`  | 
| mlp\_dim | The dimension of the output from multilayer perceptron \(MLP\) layers\. **Optional** Valid values: 2 ≤ integer ≤ 10000 Default value: 512  | 
| mlp\_layers | The number of multilayer perceptron \(MLP\) layers in the network\. **Optional** Valid values: 0 ≤ integer ≤ 10 Default value: 2  | 
| num\_classes | The number of classes for classification training\. Ignored for regression problems\. **Optional** Valid values: 2 ≤ integer ≤ 30 Default value: 2  | 
| optimizer | The optimizer type\. **Optional** Valid values: One of `adadelta`, `adagrad`, `adam`, `sgd`, or `rmsprop`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/object2vec-hyperparameters.html) Default value: `sgd` | 
| output\_layer | The type of output layer\. **Optional** Valid values: `softmax` or `mean_squared_error`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/object2vec-hyperparameters.html) Default value: `softmax`  | 
| weight\_decay | The weight decay parameter used for optimization\. **Optional** Valid values: 0 ≤ float ≤ 10000 Default value: 0  | 