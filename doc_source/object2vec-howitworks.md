# How Object2Vec Works<a name="object2vec-howitworks"></a>

When using the Amazon SageMaker Object2Vec algorithm, you follow the standard workflow: process the data, train the model, and produce inferences\. 

**Topics**
+ [Step 1: Process Data](#object2vec-step-1-data-preprocessing)
+ [Step 2: Train a Model](#object2vec-step-2-training-model)
+ [Step 3: Produce Inferences](#object2vec-step-3-inference)

## Step 1: Process Data<a name="object2vec-step-1-data-preprocessing"></a>

During preprocessing, convert the data to the [JSON Lines](http://jsonlines.org/) text file format specified in [ Data Formats for Object2Vec Training](object2vec-training-formats.md) \. To get the highest accuracy during training, also randomly shuffle the data before feeding it into the model\. How you generate random permutations depends on the language\. For python, you could use `np.random.shuffle`; for Unix, `shuf`\.

## Step 2: Train a Model<a name="object2vec-step-2-training-model"></a>

The SageMaker Object2Vec algorithm has the following main components:
+ **Two input channels** – The input channels take a pair of objects of the same or different types as inputs, and pass them to independent and customizable encoders\.
+ **Two encoders** – The two encoders, enc0 and enc1, convert each object into a fixed\-length embedding vector\. The encoded embeddings of the objects in the pair are then passed into a comparator\.
+ **A comparator** – The comparator compares the embeddings in different ways and outputs scores that indicate the strength of the relationship between the paired objects\. In the output score for a sentence pair\. For example, 1 indicates a strong relationship between a sentence pair, and 0 represents a weak relationship\. 

During training, the algorithm accepts pairs of objects and their relationship labels or scores as inputs\. The objects in each pair can be of different types, as described earlier\. If the inputs to both encoders are composed of the same token\-level units, you can use a shared token embedding layer by setting the `tied_token_embedding_weight` hyperparameter to `True` when you create the training job\. This is possible, for example, when comparing sentences that both have word token\-level units\. To generate negative samples at a specified rate, set the `negative_sampling_rate` hyperparameter to the desired ratio of negative to positive samples\. This hyperparameter expedites learning how to discriminate between the positive samples observed in the training data and the negative samples that are not likely to be observed\. 

Pairs of objects are passed through independent, customizable encoders that are compatible with the input types of corresponding objects\. The encoders convert each object in a pair into a fixed\-length embedding vector of equal length\. The pair of vectors are passed to a comparator operator, which assembles the vectors into a single vector using the value specified in the he `comparator_list` hyperparameter\. The assembled vector then passes through a multilayer perceptron \(MLP\) layer, which produces an output that the loss function compares with the labels that you provided\. This comparison evaluates the strength of the relationship between the objects in the pair as predicted by the model\. The following figure shows this workflow\.

![\[Architecture of the Object2Vec Algorithm from Data Inputs to Scores\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/object2vec-training-image.png)

## Step 3: Produce Inferences<a name="object2vec-step-3-inference"></a>

After the model is trained, you can use the trained encoder to preprocess input objects or to perform two types of inference:
+ To convert singleton input objects into fixed\-length embeddings using the corresponding encoder
+ To predict the relationship label or score between a pair of input objects

The inference server automatically figures out which of the types is requested based on the input data\. To get the embeddings as output, provide only one input\. To predict the relationship label or score, provide both inputs in the pair\.