# Additional Information for Scripts<a name="container-information-add"></a>

Scripts can assign values for the hyperparameters of an algorithm\. The interpreter passes all hyperparameters specified in the training job to the entry point as script arguments\. For example, it passes the training job hyperparameters as follow\.:

```
{"HyperParameters": {"batch-size": 256, "learning-rate": 0.0001, "communicator": "pure_nccl"}}
```

When an entry point needs additional information from the container that isn't available in hyperparameters, Amazon SageMaker Containers writes this information as environment variables that are available in the script\. For example, the following training job includes the `training` and `testing` channels\.

```
from sagemaker.pytorch import PyTorch

estimator = PyTorch(entry_point='train.py', ...)

estimator.fit({'training': 's3://bucket/path/to/training/data',
               'testing': 's3://bucket/path/to/testing/data'})
```

The environment variable `SM\_CHANNEL\_{channel_name}` provides the path where the channel is located\.

```
import argparse
import os

if __name__ == '__main__':
  parser = argparse.ArgumentParser()

  ...

  # reads input channels training and testing from the environment variables
  parser.add_argument('--training', type=str, default=os.environ['SM_CHANNEL_TRAINING'])
  parser.add_argument('--testing', type=str, default=os.environ['SM_CHANNEL_TESTING'])

  args = parser.parse_args()
  ...
```