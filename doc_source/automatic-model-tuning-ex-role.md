# Get the Amazon SageMaker Execution Role<a name="automatic-model-tuning-ex-role"></a>

Get the execution role for the notebook instance\. This is the IAM role that you created when you created your notebook instance\. You pass the role to the tuning job\. 

```
from sagemaker import get_execution_role

role = sagemaker.get_execution_role()
print(role)
```

## Next Step<a name="automatic-model-tuning-ex-next-bucket"></a>

[Specify a Bucket and Data Output Location](automatic-model-tuning-ex-bucket.md)