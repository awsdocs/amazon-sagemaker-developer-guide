# Vector Enrichment Jobs<a name="geospatial-vej"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

A Vector Enrichment Job \(VEJ\) performs operations on your vector data\. Currently, you can use a VEJ to do reverse geocoding or map matching\.
<a name="geospatial-vej-rev-geo"></a>
**Reverse Geocoding**  
With a reverse geocoding VEJ, you can convert geographic coordinates \(latitude, longitude\) to human\-readable addresses powered by Amazon Location Service\. When you upload a CSV file containing the longitude and latitude coordinates, a it returns the address number, country, label, municipality, neighborhood, postal code and region of that location\. The output file consists of your input data along with columns containing these the values appended at the end\. These jobs are optimized to accept tens of thousands of GPS traces\. 
<a name="geospatial-vej-map-match"></a>
**Map Matching**  
Map matching allows you to snap GPS coordinates to road segments\. The input should be a CSV file containing the trace ID \(route\), longitude, latitude and the timestamp attributes\. There can be multiple GPS co\-ordinates per route\. The input can contain multiple routes too\. The output is a GeoJSON file that contains links of the predicted route\. It also has the snap points provided in the input\. These jobs are optimized to accept tens of thousands of drives in one request\. Map matching is supported by [OpenStreetMap](https://www.openstreetmap.org/)\. Map matching fails if the names in the input source field don't match the ones in `MapMatchingConfig`\. The error message you receive contains the the field names present in the input file and the expected field name that is not found in `MapMatchingConfig`\. 

While you need to use an Amazon SageMaker Studio notebook to execute a VEJ, you can view all the jobs you create using the UI\. To use the visualization in the notebook, you first need to export your output to your S3 bucket\. The VEJ actions you can perform are as follows\.
+ [StartVectorEnrichmentJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_StartVectorEnrichmentJob.html)
+ [GetVectorEnrichmentJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_GetVectorEnrichmentJob.html)
+ [ListVectorEnrichmentJobs](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_ListVectorEnrichmentJobs.html)
+ [StopVectorEnrichmentJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_StopVectorEnrichmentJob.html)
+ [DeleteVectorEnrichmentJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_geospatial_DeleteVectorEnrichmentJob.html)