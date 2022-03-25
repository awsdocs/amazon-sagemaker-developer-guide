# Inference Container Images<a name="neo-deployment-hosting-services-container-images"></a>

Based on your use case, replace the highlighted portion in the inference image URI template provided below with appropriate values\. 

## Amazon SageMaker XGBoost<a name="inference-container-collapse-xgboost"></a>

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/xgboost-neo:latest
```

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\.

## Keras<a name="inference-container-collapse-keras"></a>

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/sagemaker-neo-keras:fx_version-instance_type-py3
```

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\.

Replace *fx\_version* with `2.2.4`\.

Replace *instance\_type* with either `cpu` or `gpu`\.

## MXNet<a name="inference-container-collapse-mxnet"></a>

------
#### [ CPU or GPU instance types ]

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/sagemaker-inference-mxnet:fx_version-instance_type-py3
```

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\. 

Replace *fx\_version* with `1.8.0`\. 

Replace *instance\_type* with either `cpu` or `gpu`\. 

------
#### [ Inferentia instance types ]

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/sagemaker-neo-mxnet:fx_version-instance_type-py3
```

Replace *aws\_region* with either `us-east-1` or `us-west-2`\. 

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\. 

Replace *fx\_version* with `1.5.1`\. 

Replace *`instance_type`* with `inf`\.

------

## ONNX<a name="inference-container-collapse-keras"></a>

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/sagemaker-neo-onnx:fx_version-instance_type-py3
```

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\.

Replace *fx\_version* with `1.5.0`\.

Replace *instance\_type* with either `cpu` or `gpu`\.

## PyTorch<a name="inference-container-collapse-pytorch"></a>

------
#### [ CPU or GPU instance types ]

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/sagemaker-inference-pytorch:fx_version-instance_type-py3
```

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\. 

Replace *fx\_version* with `1.4`, `1.5`, `1.6`, `1.7` or `1.8`\.

Replace *instance\_type* with either `cpu` or `gpu`\. 

------
#### [ Inferentia instance types ]

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/sagemaker-neo-pytorch:fx_version-instance_type-py3
```

Replace *aws\_region* with either `us-east-1` or `us-west-2`\. 

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\. 

Replace *fx\_version* with `1.5.1`\. 

Replace *`instance_type`* with `inf`\.

------

## TensorFlow<a name="inference-container-collapse-tf"></a>

------
#### [ CPU or GPU instance types ]

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/sagemaker-inference-tensorflow:fx_version-instance_type-py3
```

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\. 

Replace *fx\_version* with `1.15.3` or `2.4`\. 

Replace *instance\_type* with either `cpu` or `gpu`\. 

------
#### [ Inferentia instance types ]

```
aws_account_id.dkr.ecr.aws_region.amazonaws.com/sagemaker-neo-tensorflow:fx_version-instance_type-py3
```

Replace *aws\_account\_id* from the table at the end of this page based on the *aws\_region* you used\. Note that for instance type `inf` only `us-east-1` and `us-west-2` are supported\.

Replace *fx\_version* with `1.15.0`

Replace *instance\_type* with `inf`\.

------

The following table maps *aws\_account\_id* with *aws\_region*\. Use this table to find the correct inference image URI you need for your application\. 


| aws\_account\_id | aws\_region | 
| --- | --- | 
| 785573368785 | us\-east\-1 | 
| 007439368137 | us\-east\-2 | 
| 710691900526 | us\-west\-1 | 
| 301217895009 | us\-west\-2 | 
| 802834080501 | eu\-west\-1 | 
| 205493899709 | eu\-west\-2 | 
| 254080097072 | eu\-west\-3 | 
| 601324751636 | eu\-north\-1 | 
| 966458181534 | eu\-south\-1 | 
| 746233611703 | eu\-central\-1 | 
| 110948597952 | ap\-east\-1 | 
| 763008648453 | ap\-south\-1 | 
| 941853720454 | ap\-northeast\-1 | 
| 151534178276 | ap\-northeast\-2 | 
| 324986816169 | ap\-southeast\-1 | 
| 355873309152 | ap\-southeast\-2 | 
| 474822919863 | cn\-northwest\-1 | 
| 472730292857 | cn\-north\-1 | 
| 756306329178 | sa\-east\-1 | 
| 464438896020 | ca\-central\-1 | 
| 836785723513 | me\-south\-1 | 
| 774647643957 | af\-south\-1 | 