# Make Real\-time Predictions with an Inference Pipeline<a name="inference-pipeline-real-time"></a>

Trained models can be used in an inference pipeline for making real\-time predictions directly without any external pre\-processing\. As part of the pipeline, customers can choose to use the built\-in feature transformers already available in Amazon SageMaker, or they can implement their own transformation logic using just a few lines of scikit\-learn or Spark code\. This section contains examples for each of these cases\.

[MLeap](http://mleap-docs.combust.ml/) is a serialization format and execution engine for machine learning pipelines\. It supports Spark, scikit\-learn, and TensorFlow for training pipelines and exporting them to a serialized pipeline called an MLeap Bundle\. Bundles can be deserialized back into Spark for batch\-mode scoring or the MLeap runtime to power real\-time API services\.

The containers in the pipeline need to listen on the port specified by the SAGEMAKER\_BIND\_TO\_PORT environment variable \(instead of 8080\)\. This environment variable is automatically provided to containers when running in an inference pipeline\. If this environment variable is not present, containers should default to 8080\. Use the following command to add a label to your Dockerfile that indicates that your container complies with this requirement:

```
LABEL com.amazonaws.sagemaker.capabilities.accept-bind-to-port=true
```

If your container needs to listen on a second port, choose a port in the range specified by the SAGEMAKER\_SAFE\_PORT\_RANGE environment variable\. The value is specify an inclusive range in the format "XXXX\-YYYY", where XXXX and YYYY are two multi\-digit integers\. This is also provided automatically when running in a multicontainer pipeline\.

**Note**  
When your own custom Docker images are used in a pipeline that includes [Amazon SageMaker built\-in algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html), we require an [Amazon ECR policy](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)\. In particular, your repo needs to grant permission for Amazon SageMaker to pull the image\. For information on how to implement this ECR policy, see [Troubleshoot ECR Permissions for Inference Pipelines](inference-pipeline-troubleshoot.md#inference-pipeline-troubleshoot-permissions)\.

## Create and Deploy an Inference Pipeline Endpoint<a name="inference-pipeline-real-time-sdk"></a>

The following code shows how to create and deploy a real\-time inference pipeline model with SparkML and XGBoost models in series using the Amazon SageMaker SDK\.

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

## Request an Inference from an Endpoint<a name="inference-pipeline-endpoint-request"></a>

The following code shows how to make predictions by calling a real\-time inference endpoint and passing a request payload in JSON format:

```
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

predictor = RealTimePredictor(endpoint=endpoint_name, sagemaker_session=sess, serializer=json_serializer,
                                content_type=CONTENT_TYPE_JSON, accept=CONTENT_TYPE_CSV)

print(predictor.predict(payload))
```