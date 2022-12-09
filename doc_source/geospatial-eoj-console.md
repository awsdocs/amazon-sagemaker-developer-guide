# Create an Earth Observation Job Using SageMaker geospatial UI<a name="geospatial-eoj-console"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

To create an Earth Observation job \(EOJ\) using the SageMaker geospatial UI, do the following\.

1. In the **Geospatial** tab of Amazon SageMaker Studio, choose **Earth Observation jobs**\.

1. On the **Earth Observation jobs** page, choose **Create job**\.

1. A page titled **Create an Earth Observation job** opens\. The page includes fields for **Job name** and **Choose a model**\. Enter the following information:

   1. **Job name**– Must contain a maximum of 63 alphanumeric characters\. Can include hyphens \(\-\) but not spaces\. 

   1. **Choose a model**– From the models available for your data, choose any one\. See [Types of Models](https://docs.aws.amazon.com/) to learn more about the models\.

1. Select **Next**\.

1. Now, you need to define the geographical area you want to analyze as an Area of Interest \(AoI\)\. The AoI is provided to the satellite image collections as input\. To define the AoI, you can upload a GeoJSON file\.
**Note**  
The input GeoJSON file should contain a single GeoJSON polygon geometry\.

1. After you define your AoI, you select an image collection from the right panel\. You can also set a date range and cloud coverage percentage as a filter\.

1. If you select **Spectral Index** as a model to create the EOJ, you can upload Planet data from your Amazon S3 bucket\. You need to provide the S3 location for your [order manifest](https://developers.planet.com/apis/orders/delivery/) file\. You also need to select or create an IAM role that has the [AmazonSageMakerExecutionRole](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html) policy attached to it\.

1. Choose **Create**\.

The following is an example of a single GeoJSON polygon geometry where Seattle is the AoI\. Copy the following code and save it in a file with the extension `.geojson`\. You can use this file as an example AoI when you create an EOJ\.

```
{
    "type": "Polygon",
    "coordinates": [
            [
              [-122.4258, 47.5116], 
              [-122.2459, 47.5116], 
              [-122.2459, 47.7429], 
              [-122.4258, 47.7429], 
              [-122.4258, 47.5116]
            ]
    ]
  }
```