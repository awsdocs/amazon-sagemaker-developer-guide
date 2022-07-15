# Upgrade XGBoost Version 0\.90 to Version 1\.5<a name="xgboost-version-0.90"></a>

If you are using the SageMaker Python SDK, to upgrade existing XGBoost 0\.90 jobs to version 1\.5, you must have version 2\.x of the SDK installed and change the XGBoost `version` and `framework_version` parameters to 1\.5\-1\. If you are using Boto3, you need to update the Docker image, and a few hyperparameters and learning objectives\.

**Topics**
+ [Upgrade SageMaker Python SDK Version 1\.x to Version 2\.x](#upgrade-xgboost-version-0.90-SageMaker-python-sdk)
+ [Change the image tag to 1\.5\-1](#upgrade-xgboost-version-0.90-change-image-tag)
+ [Change Docker Image for Boto3](#upgrade-xgboost-version-0.90-boto3)
+ [Update Hyperparameters and Learning Objectives](#upgrade-xgboost-version-0.90-hyperparameters)

## Upgrade SageMaker Python SDK Version 1\.x to Version 2\.x<a name="upgrade-xgboost-version-0.90-SageMaker-python-sdk"></a>

If you are still using Version 1\.x of the SageMaker Python SDK, you must to upgrade version 2\.x of the SageMaker Python SDK\. For information on the latest version of the SageMaker Python SDK, see [Use Version 2\.x of the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/v2.html)\. To install the latest version, run:

```
python -m pip install --upgrade sagemaker
```

## Change the image tag to 1\.5\-1<a name="upgrade-xgboost-version-0.90-change-image-tag"></a>

If you are using the SageMaker Python SDK and using the XGBoost build\-in algorithm, change the version parameter in `image_uris.retrive`\.

```
from sagemaker import image_uris
image_uris.retrieve(framework="xgboost", region="us-west-2", version="1.5-1")

estimator = sagemaker.estimator.Estimator(image_uri=xgboost_container, 
                                          hyperparameters=hyperparameters,
                                          role=sagemaker.get_execution_role(),
                                          instance_count=1, 
                                          instance_type='ml.m5.2xlarge', 
                                          volume_size=5, # 5 GB 
                                          output_path=output_path)
```

If you are using the SageMaker Python SDK and using XGBoost as a framework to run your customized training scripts, change the `framework_version` parameter in the XGBoost API\.

```
estimator = XGBoost(entry_point = "your_xgboost_abalone_script.py", 
                    framework_version='1.5-1',
                    hyperparameters=hyperparameters,
                    role=sagemaker.get_execution_role(),
                    instance_count=1,
                    instance_type='ml.m5.2xlarge',
                    output_path=output_path)
```

`sagemaker.session.s3_input` in SageMaker Python SDK version 1\.x has been renamed to `sagemaker.inputs.TrainingInput`\. You must use `sagemaker.inputs.TrainingInput` as in the following example\.

```
content_type = "libsvm"
train_input = TrainingInput("s3://{}/{}/{}/".format(bucket, prefix, 'train'), content_type=content_type)
validation_input = TrainingInput("s3://{}/{}/{}/".format(bucket, prefix, 'validation'), content_type=content_type)
```

 For the full list of SageMaker Python SDK version 2\.x changes, see [Use Version 2\.x of the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/v2.html)\. 

## Change Docker Image for Boto3<a name="upgrade-xgboost-version-0.90-boto3"></a>

If you are using Boto3 to train or deploy your model, change the docker image tag \(1, 0\.72, 0\.90\-1 or 0\.90\-2\) to 1\.5\-1\.

```
{
    "AlgorithmSpecification":: {
        "TrainingImage": "746614075791.dkr.ecr.us-west-1.amazonaws.com/sagemaker-xgboost:1.5-1"
    }
    ...
}
```

If you using the SageMaker Python SDK to retrieve registry path, change the `version` parameter in `image_uris.retrieve`\.

```
from sagemaker import image_uris
image_uris.retrieve(framework="xgboost", region="us-west-2", version="1.5-1")
```

## Update Hyperparameters and Learning Objectives<a name="upgrade-xgboost-version-0.90-hyperparameters"></a>

The silent parameter has been deprecated and is no longer available in XGBoost 1\.5 and later versions\. Use `verbosity` instead\. If you were using the `reg:linear` learning objective, it has been deprecated as well in favor of` reg:squarederror`\. Use `reg:squarederror` instead\.

```
hyperparameters = {
    "verbosity": "2",
    "objective": "reg:squarederror",
    "num_round": "50",
    ...
}

estimator = sagemaker.estimator.Estimator(image_uri=xgboost_container, 
                                          hyperparameters=hyperparameters,
                                          ...)
```