# Amazon SageMaker geospatial capabilities<a name="geospatial"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

 Amazon SageMaker geospatial capabilities make it easier for data scientists and machine learning \(ML\) engineers to build, train, and deploy ML models faster using geospatial data\. You have access to open\-source and third\-party data, processing, and visualization tools to make it more efficient to prepare geospatial data for ML\. You can increase your productivity by using purpose\-built algorithms and pre\-trained ML models to speed up model building and training, and use built\-in visualization tools to explore prediction outputs on an interactive map and then collaborate across teams on insights and results\. 
<a name="why-use-geo"></a>
**Why use SageMaker geospatial capabilities?**  
You can use SageMaker geospatial capabilities to make predictions on geospatial data faster than do\-it\-yourself solutions\. SageMaker geospatial capabilities make it easier to access geospatial data from your existing customer data lakes, open\-source datasets, and other SageMaker geospatial data providers\. SageMaker geospatial capabilities minimize the need for building custom infrastructure and data preprocessing functions by offering purpose\-built algorithms for efficient data preparation, model training, and inference\. You can also create and share custom visualizations and data with your company from Amazon SageMaker Studio\. SageMaker geospatial capabilities offer pre\-trained models for common uses in agriculture, real estate, insurance, and financial services\.

**Note**  
Currently, SageMaker geospatial capabilities are only supported in the US West \(Oregon\) Region for public preview\.
<a name="how-use-geo"></a>
**How can I use SageMaker geospatial capabilities?**  
You can use SageMaker geospatial capabilities in two ways\.
+ Through the SageMaker geospatial UI, as a part of Amazon SageMaker Studio UI\.
+ Through SageMaker notebooks with a SageMaker geospatial image\.

Geospatial data represents features or objects on the earth’s surface\. The first type of geospatial data is vector data, which uses two\-dimensional geometry such as points, lines, or polygons to represent objects such as roads and land boundaries\. The second type of geospatial data is raster data, such as imagery captured by satellite, aerial platforms, or remote sensing data\. This data type uses a matrix of pixels to define where features are located\. You can use raster formats for storing data that varies\. A third type of geospatial data is geo\-tagged location data\. It includes points of interest—for example, the Eiffel Tower—location\-tagged social media posts, latitude and longitude coordinates, or different styles and formats of street addresses\. SageMaker has the following SageMaker geospatial capabilities\.
+ An [Earth Observation job](https://docs.aws.amazon.com/sagemaker/latest/dg/geospatial-eoj.html) for raster data
+ A [Vector Enrichment job](https://docs.aws.amazon.com/sagemaker/latest/dg/geospatial-vej.html) for vector data
+ Built\-in [Visualization Using SageMaker geospatial capabilities](https://docs.aws.amazon.com/sagemaker/latest/dg/geospatial-visualize.html)
+ A geospatial image that has commonly used open\-source libraries used in the geospatial ML workflow pre\-installed

Along with this, you can access data from a catalog of geospatial data providers\. Currently, the data collections available include: 
+ [ USGS Landsat](https://www.usgs.gov/centers/eros/data-citation?qt-science_support_page_related_con=0#qt-science_support_page_related_con)
+ [Sentinel\-2](https://sentinel.esa.int/web/sentinel/missions/sentinel-2)

You can also bring your own [Planet](https://www.planet.com/) data by ordering it\. Planet is one of the leading daily satellite data providers\. After Planet delivers the data to you, you need to provide the [order manifest](https://developers.planet.com/apis/orders/delivery/) file included in the Planet data to SageMaker\.

**Topics**
+ [Getting Started with Amazon SageMaker geospatial capabilities](geospatial-getting-started.md)
+ [Earth Observation Jobs](geospatial-eoj.md)
+ [Vector Enrichment Jobs](geospatial-vej.md)
+ [Visualization Using SageMaker geospatial capabilities](geospatial-visualize.md)
+ [Amazon SageMaker geospatial Map SDK](geospatial-notebook-sdk.md)