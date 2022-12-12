# Types of Compute Instances<a name="geospatial-instances"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

SageMaker geospatial capabilities offer three types of compute instances\. Here's how you can use them:
+ **ml\.geospatial\.interactive** – Launch an interactive SageMaker notebook with a SageMaker geospatial image to build, train, and deploy ML models\.
+ **ml\.geospatial\.jobs** – Run processing jobs to transform satellite image data\.
+ **ml\.geospatial\.models** – Make predictions using pre\-trained ML models on satellite imagery\.

The instance type is determined by the operation that you run\. The following table shows the instance type for each operation\.


| Operations |  Instance  | 
| --- | --- | 
| Launch a SageMaker Studio notebook with SageMaker geospatial image | ml\.geospatial\.interactive | 
| Temporal Statistics | ml\.geospatial\.jobs | 
| Zonal Statistics | ml\.geospatial\.jobs | 
| Resampling | ml\.geospatial\.jobs | 
| Geomosaic | ml\.geospatial\.jobs | 
| Band Stacking | ml\.geospatial\.jobs | 
| Band Math | ml\.geospatial\.jobs | 
| Cloud Removal with Landsat8 | ml\.geospatial\.jobs | 
| Cloud Removal with Sentinel\-2 | ml\.geospatial\.models | 
| Cloud Masking | ml\.geospatial\.models | 
| Land Cover Segmentation | ml\.geospatial\.models | 

You are charged different rates for each type of compute instance you use\. See [Geospatial ML with Amazon SageMaker](http://aws.amazon.com/sagemaker/geospatial) for more information on pricing\.