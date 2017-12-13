# Step 3\.2\.1: Download the MNIST Dataset<a name="ex1-preprocess-data-pull-data"></a>

To download the MNIST dataset, copy and paste the following code into the notebook and run it: 

```
%%time
import pickle, gzip, numpy, urllib.request, json

# Load the dataset
urllib.request.urlretrieve("http://deeplearning.net/data/mnist/mnist.pkl.gz", "mnist.pkl.gz")
with gzip.open('mnist.pkl.gz', 'rb') as f:
    train_set, valid_set, test_set = pickle.load(f, encoding='latin1')
```

The code does the following:

1. Downloads the MNIST dataset \(`mnist.pkl.gz`\) from the deeplearning\.net website to your Amazon SageMaker notebook instance\. 

1. Unzips the file and reads the following three datasets into the notebook's memory:

   + `train_set`—You use these images of handwritten numbers to train a model\.

   + `valid_set`—After you train the model, you validate it using the images in this dataset\. 

   + `test_set`—You don't use this dataset in this exercise\.

**Next Step**  
[Step 3\.2\.2: Explore the Training Dataset](ex1-preprocess-data-inspect.md)