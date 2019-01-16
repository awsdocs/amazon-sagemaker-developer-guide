# How Object2Vec Works<a name="object2vec-howitworks"></a>

## Step 1: Data Processing<a name="object2vec-step-1-data-preprocessing"></a>

The data must be converted to the [JSON Lines](http://jsonlines.org/) text file format specified in the [ Data Formats for Object2Vec Training](object2vec-training-formats.md) section during preprocessing\. Also, to ensure obtaining the best accuracy from training, the data needs to be randomly shuffled before feeding it into the model\.

## Step 2: Model Training<a name="object2vec-step-2-training-model"></a>

The architecture of Amazon SageMaker Object2Vec consists of following main components:
+ *Two input channels*—The 2 input channels take a pair of objects of same or different types as inputs, and pass them to independent and customizable encoders\. An example of the input objects could be sequence pairs, tokens pairs, or sequence and tokens pairs\.
+ *Two encoders*: The enc0 and enc1 encoders convert each object into a fixed\-length embedding vector\. The encoded embeddings of the objects in the pair, which are then passed into a comparator\.
+ *Comparator*—The comparator compares the embeddings in different ways and outputs scores that indicate the strength of the relationship between the paired objects for each user\-specified relationship\-type\. The output scores can vary between 1, indicating a strong relationship between a sentence pair, and 0, representing a weak relationship\. 

At training time, the algorithm accepts pairs of objects and their relationship labels or scores as inputs\. The objects in each pair can be of different types\. They are passed through independent, customizable encoders that are compatible with the input types of corresponding objects\. The encoders convert each object in a pair into a fixed\-length embedding vector of equal length\. The encoded embeddings of the objects in the pair are then passed into a comparator that compares the embeddings in different ways and outputs scores\. The scores correspond to the strength of the relationship between the objects in the pair for each relationship\-type specified by the user\. The training loss function can then minimize the disagreement between the relationships predicted by the model and those specified by the user\-provided labels in the training data\. 

![\[General Architecture of Object2Vec: from data inputs to scores.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/object2vec-training-image.png)

## Step 3: Inference<a name="object2vec-step-3-inference"></a>

After the model has been trained, you can use the trained encoder to perform inference in two modes:
+ To convert singleton input objects into fixed length embeddings using the corresponding encoder
+ To predict the relationship label or score between a pair of input objects

The inference server automatically figures out which of the modes is requested based on the input data\. To get the embeddings as output, provide only one input in each instance\. To predict the relationship label or score, provide both inputs in the pair\.