# Step 4\.1: Download the MNIST Dataset<a name="ex1-preprocess-data-pull-data"></a>

To download the MNIST dataset, copy and paste the following code into the notebook and run it:\.

```
%%time 
import pickle, gzip, urllib.request, json
import numpy as np

# Load the dataset
urllib.request.urlretrieve("http://deeplearning.net/data/mnist/mnist.pkl.gz", "mnist.pkl.gz")
with gzip.open('mnist.pkl.gz', 'rb') as f:
    train_set, valid_set, test_set = pickle.load(f, encoding='latin1')
print(train_set[0].shape)
```

The code does the following:

1. Downloads the MNIST dataset \(`mnist.pkl.gz`\) from the MNIST Database website to your notebook\. 

1. Unzips the file and reads the following datasets into the notebook's memory:
   + `train_set` – You use these images of handwritten numbers to train a model\.
   + `valid_set` – The [XGBoost Algorithm](xgboost.md) uses these images to evaluate the progress of the model during training\.
   + `test_set` – You use this set to get inferences to test the deployed model\.

**Next Step**  
[Step 4\.2: Explore the Training Dataset](ex1-preprocess-data-inspect.md)