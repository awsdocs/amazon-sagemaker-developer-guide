# Run Real\-time Predictions with an Inference Pipeline<a name="inference-pipeline-real-time"></a>

You can use trained models in an inference pipeline to make real\-time predictions directly without performing external preprocessing\. When you configure the pipeline, you can choose to use the built\-in feature transformers already available in Amazon SageMaker\. Or, you can implement your own transformation logic using just a few lines of scikit\-learn or Spark code\. 

[MLeap](http://mleap-docs.combust.ml/), a serialization format and execution engine for machine learning pipelines, supports Spark, scikit\-learn, and TensorFlow for training pipelines and exporting them to a serialized pipeline called an MLeap Bundle\. You can deserialize Bundles back into Spark for batch\-mode scoring or into the MLeap runtime to power real\-time API services\.

The containers in a pipeline listen on the port specified in the `SAGEMAKER_BIND_TO_PORT` environment variable \(instead of 8080\)\. When running in an inference pipeline, SageMaker automatically provides this environment variable to containers\. If this environment variable isn't present, containers default to using port 8080\. To indicate that your container complies with this requirement, use the following command to add a label to your Dockerfile:

```
LABEL com.amazonaws.sagemaker.capabilities.accept-bind-to-port=true
```

If your container needs to listen on a second port, choose a port in the range specified by the `SAGEMAKER_SAFE_PORT_RANGE` environment variable\. Specify the value as an inclusive range in the format **"XXXX\-YYYY"**, where `XXXX` and `YYYY` are multi\-digit integers\. SageMaker provides this value automatically when you run the container in a multicontainer pipeline\.

**Note**  
To use custom Docker images in a pipeline that includes [SageMaker built\-in algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html), you need an [Amazon Elastic Container Registry \(Amazon ECR\) policy](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)\. Your Amazon ECR repository must grant SageMaker permission to pull the image\. For more information, see [Troubleshoot Amazon ECR Permissions for Inference Pipelines](inference-pipeline-troubleshoot.md#inference-pipeline-troubleshoot-permissions)\.

## Create and Deploy an Inference Pipeline Endpoint<a name="inference-pipeline-real-time-sdk"></a>

The following code creates and deploys a real\-time inference pipeline model with SparkML and XGBoost models in series using the SageMaker SDK\.

```
from sagemaker.model import Model
from sagemaker.pipeline_model import PipelineModel
from sagemaker.sparkml.model import SparkMLModel

sparkml_data = 's3://{}/{}/{}'.format(s3_model_bucket, s3_model_key_prefix, 'model.tar.gz')
sparkml_model = SparkMLModel(model_data=sparkml_data)
xgb_model = Model(model_data=xgb_model.model_data, image=training_image)

model_name = 'serial-inference-' + timestamp_prefix
endpoint_name = 'serial-inference-ep-' + timestamp_prefix
sm_model = PipelineModel(name=model_name, role=role, models=[sparkml_model, xgb_model])
sm_model.deploy(initial_instance_count=1, instance_type='ml.c4.xlarge', endpoint_name=endpoint_name)
```

## Request Real\-Time Inference from an Inference Pipeline Endpoint<a name="inference-pipeline-endpoint-request"></a>

The following example shows how to make real\-time predictions by calling an inference endpoint and passing a request payload in JSON format:

```
import sagemaker
from sagemaker.predictor import json_serializer, json_deserializer, RealTimePredictor
from sagemaker.content_types import CONTENT_TYPE_CSV, CONTENT_TYPE_JSON

payload = {
        "input": [
            {
                "name": "Pclass",
                "type": "float",
                "val": "1.0"
            },
            {
                "name": "Embarked",
                "type": "string",
                "val": "Q"
            },
            {
                "name": "Age",
                "type": "double",
                "val": "48.0"
            },
            {
                "name": "Fare",
                "type": "double",
                "val": "100.67"
            },
            {
                "name": "SibSp",
                "type": "double",
                "val": "1.0"
            },
            {
                "name": "Sex",
                "type": "string",
                "val": "male"
            }
        ],
        "output": {
            "name": "features",
            "type": "double",
            "struct": "vector"
        }
    }

predictor = RealTimePredictor(endpoint=endpoint_name, sagemaker_session=sagemaker.Session(), serializer=json_serializer,
                                content_type=CONTENT_TYPE_JSON, accept=CONTENT_TYPE_CSV)

print(predictor.predict(payload))
```

The response you get from `predictor.predict(payload)` is the model's inference result\.

## Realtime inference pipeline example<a name="inference-pipeline-example"></a>

You can run this [example notebook using the SKLearn predictor](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/scikit_learn_randomforest/Sklearn_on_SageMaker_end2end.ipynb) that shows how to deploy an endpoint, run an inference request, then deserialize the response\. Find this notebook and more examples in the [Amazon SageMaker example GitHub repository](https://github.com/awslabs/amazon-sagemaker-examples)\.