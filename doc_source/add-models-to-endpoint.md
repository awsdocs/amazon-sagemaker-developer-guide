# Add or Remove Models<a name="add-models-to-endpoint"></a>

You can deploy additional models to a multi\-model endpoint and invoke them through that endpoint immediately\. When adding a new model, you don't need to update or bring down the endpoint, so you avoid the cost of creating and running a separate endpoint for each new model\. 

 SageMaker unloads unused models from the container when the instance is reaching memory capacity and more models need to be downloaded into the container\. SageMaker also deletes unused model artifacts from the instance storage volume when the volume is reaching capacity and new models need to be downloaded\. The first invocation to a newly added model takes longer because the endpoint takes time to download the model from S3 to the container's memory in instance hosting the endpoint

With the endpoint already running, copy a new set of model artifacts to the Amazon S3 location there you store your models\.

```
# Add an AdditionalModel to the endpoint and exercise it
aws s3 cp AdditionalModel.tar.gz s3://my-bucket/path/to/artifacts/
```

**Important**  
To update a model, proceed as you would when adding a new model\. Use a new and unique name\. Don't overwrite model artifacts in Amazon S3 because the old version of the model might still be loaded in the containers or on the storage volume of the instances on the endpoint\. Invocations to the new model could then invoke the old version of the model\. 

Client applications can request predictions from the additional target model as soon as it is stored in S3\.

```
response = runtime_sm_client.invoke_endpoint(
                        EndpointName='endpoint_name',
                        ContentType='text/csv',
                        TargetModel='AdditionalModel.tar.gz',
                        Body=body)
```

To delete a model from a multi\-model endpoint, stop invoking the model from the clients and remove it from the S3 location where model artifacts are stored\.