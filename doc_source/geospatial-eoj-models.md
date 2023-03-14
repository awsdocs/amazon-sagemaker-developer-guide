# Types of Operations<a name="geospatial-eoj-models"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

When you create an EOJ, you select an operation based on your use case\. Amazon SageMaker geospatial capabilities provide a combination of purpose\-built operations and pre\-trained models\. You can use these operations to understand the impact of environmental changes and human activities over time or identify cloud and cloud\-free pixels\.

**Cloud Masking**

Identify clouds in satellite images is an essential pre\-processing step in producing high\-quality geospatial data\. Ignoring cloud pixels can lead to errors in analysis, and over\-detection of cloud pixels can decrease the number of valid observations\. Cloud masking has the ability to identify cloudy and cloud\-free pixels in satellite images\. An accurate cloud mask helps get satellite images for processing and improves data generation\. The following is the class map for cloud masking\.

```
{
0: "No_cloud",
1: "cloud"
}
```

**Cloud Removal**

Cloud removal for Sentinel\-2 data uses an ML\-based semantic segmentation model to identify clouds in the image\. Cloudy pixels can be replaced by with pixels from other timestamps\. USGS Landsat data contains landsat metadata that is used for cloud removal\.

**Temporal Statistics**

Temporal statistics calculate statistics for geospatial data through time\. The temporal statistics currently supported include mean, median, and standard deviation\. You can calculate these statistics by using `GROUPBY` and set it to either `all` or `yearly`\. You can also mention the `TargetBands`\.

**Zonal Statistics**

Zonal statistics performs statistical operations over a specified area on the image\. 

**Resampling**

Resampling is used to upscale and downscale the resolution of a geospatial image\. The `value` attribute in resampling represents the length of a side of the pixel\.

**Geomosaic**

Geomosaic allows you to stitch smaller images into a large image\.

**Band Stacking**

Band stacking takes more than one image band as input and stacks them into a single GeoTIFF\. The `OutputResolution` attribute determines the resolution of the output image\. Based on the resolutions of the input images, you can set it to `lowest`, `highest` or `average`\.

**Band Math**

Band Math, also known as Spectral Index, is a process of transforming the observations from multiple spectral bands to a single band, indicating the relative abundance of features of interests\. For instance, Normalized Difference Vegetation Index \(NDVI\) and Enhanced Vegetation Index \(EVI\) are helpful for observing the presence of green vegetation features\.

**Land Cover Segmentation**

Land Cover segmentation is a semantic segmentation model that has the capability to identify the physical material, such as vegetation, water, and bare ground, at the earth surface\. Having an accurate way to map the land cover patterns helps you understand the impact of environmental change and human activities over time\. Land Cover segmentation is often used for region planning, disaster response, ecological management, and environmental impact assessment\. The following is the class map for Land Cover segmentation\.

```
{
0: "No_data",
1: "Saturated_or_defective",
2: "Dark_area_pixels",
3: "Cloud_shadows",
4: "Vegetation",
5: "Not_vegetated",
6: "Water",
7: "Unclassified",
8: "Cloud_medium_probability",
9: "Cloud_high_probability",
10: "Thin_cirrus",
11: "Snow_ice"
}
```

## Availability of EOJ Operations<a name="geospatial-eoj-models-avail"></a>

The availability of operations depends on whether you are using the SageMaker geospatial UI or the Amazon SageMaker Studio notebooks with a SageMaker geospatial image\. Currently, notebooks support all functionalities\. To summarize, the following geospatial operations are supported by SageMaker:


| Operations |  Description  |  Availability  | 
| --- | --- | --- | 
| Cloud Masking | Identify cloud and cloud\-free pixels to get improved and accurate satellite imagery\. | UI, Notebook | 
| Cloud Removal | Remove pixels containing parts of a cloud from satellite imagery\. | Notebook | 
| Temporal Statistics | Calculate statistics through time for a given GeoTIFF\. | Notebook | 
| Zonal Statistics | Calculate statistics on user\-defined regions\. | Notebook | 
| Resampling | Scale images to different resolutions\. | Notebook | 
| Geomosaic | Combine multiple images for greater fidelity\. | Notebook | 
| Band Stacking | Combine multiple spectral bands to create a single image\. | Notebook | 
| Band Math / Spectral Index | Obtain a combination of spectral bands that indicate the abundance of features of interest\. | UI, Notebook | 
| Land Cover Segmentation | Identify land cover types such as vegetation and water in satellite imagery\. | UI, Notebook | 