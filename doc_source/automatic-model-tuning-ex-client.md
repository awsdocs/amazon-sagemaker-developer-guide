# Get the Amazon Sagemaker Boto 3 Client<a name="automatic-model-tuning-ex-client"></a>

Import libraries and get a Boto3 client, which you use to call the hyperparameter tuning APIs\.

In the new Jupyter notebook, type the following code:

```
import sagemaker
import boto3
from sagemaker.predictor import csv_serializer    # Converts strings for HTTP POST requests on inference

import numpy as np                                # For performing matrix operations and numerical processing
import pandas as pd                               # For manipulating tabular data
from time import gmtime, strftime
import os

region = boto3.Session().region_name
smclient = boto3.Session().client('sagemaker')
```

## Next Step<a name="automatic-model-tuning-ex-next-role"></a>

[Get the Amazon SageMaker Execution Role](automatic-model-tuning-ex-role.md)