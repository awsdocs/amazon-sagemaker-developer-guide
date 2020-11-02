# Get the Amazon SageMaker Boto 3 Client<a name="automatic-model-tuning-ex-client"></a>

Import Amazon SageMaker Python SDK, AWS SDK for Python \(Boto3\), and other Python libraries\. In a new Jupyter notebook, paste the following code to the first cell:

```
import sagemaker
import boto3

import numpy as np                                # For performing matrix operations and numerical processing
import pandas as pd                               # For manipulating tabular data
from time import gmtime, strftime
import os

region = boto3.Session().region_name
smclient = boto3.Session().client('sagemaker')
```

The preceding code cell defines `region` and `smclient` objects that you will use to call the built\-in XGBoost algorithm and set the SageMaker hyperparameter tuning job\.

## Next Step<a name="automatic-model-tuning-ex-next-role"></a>

[Get the SageMaker Execution Role](automatic-model-tuning-ex-role.md)