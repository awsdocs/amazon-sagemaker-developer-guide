# Create an Earth Observation Job Using a Amazon SageMaker Studio Notebook with a SageMaker geospatial Image<a name="geospatial-eoj-ntb"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

**To use a SageMaker Studio notebook with a SageMaker geospatial image:**

1. From the **Launcher**, choose **Change environment** under **Notebooks and compute resources**\.

1. Next, the **Change environment** dialog opens\.

1. Select the **Image** dropdown and choose **Geospatial 1\.0**\. The **Instance type** should be **ml\.m5\.4xlarge**\. Do not change the default values for other settings\.

1. Choose **Select**\.

1. Choose **Create notebook**\.

You can initiate an EOJ using a Amazon SageMaker Studio notebook with a SageMaker geospatial image using the code provided below\.

```
import boto3
import botocore
import sagemaker
import sagemaker_geospatial_map

region = boto3.Session().region_name
session = botocore.session.get_session()
execution_role = sagemaker.get_execution_role()

sg_client= session.create_client(
    service_name='sagemaker-geospatial',
    region_name=region
)
```

The following is an example showing how to create an EOJ in the in the US West \(Oregon\) Region\.

```
#Query and Access Data
data_manifest = sg_client.search_raster_data_collection(
    Arn = 'arn:aws:sagemaker-geospatial:us-west-2:378778860802:raster-data-collection/public/nmqj48dcu3g7ayw8',
    RasterDataCollectionQuery =  {
        "AreaOfInterest": {
            "AreaOfInterestGeometry": {
                "PolygonGeometry": {
                    "Coordinates": [
                        [[-122.4258, 47.5116], [-122.2459, 47.5116], [-122.2459, 47.7429], [-122.4258, 47.7429], [-122.4258, 47.5116]]
                    ]
                }
            }
        },
        "TimeRangeFilter": {
            "StartTime": "2022-11-01T00:00:00Z",
            "EndTime": "2022-11-22T23:59:59Z"
        },
        "PropertyFilters": {
            "Properties": [
                {
                    "Property": {
                        "EoCloudCover": {
                            "LowerBound": 0,
                            "UpperBound": 20
                        }
                    }
                }
            ],
            "LogicalOperator": "AND"
        }
    },
)
data_manifest

# Perform land cover segmentation on images returned from the sentinel dataset.
eoj_input_config = {
    "RasterDataCollectionQuery": {
        "RasterDataCollectionArn": "arn:aws:sagemaker-geospatial:us-west-2:378778860802:raster-data-collection/public/nmqj48dcu3g7ayw8",
        "AreaOfInterest": {
            "AreaOfInterestGeometry": {
                "PolygonGeometry": {
                    "Coordinates": [
                        [[-122.4258, 47.5116], [-122.2459, 47.5116], [-122.2459, 47.7429], [-122.4258, 47.7429], [-122.4258, 47.5116]]
                    ]
                }
            }
        },
        "TimeRangeFilter": {
            "StartTime": "2022-11-01T00:00:00Z",
            "EndTime": "2022-11-22T23:59:59Z"
        },
        "PropertyFilters": {
            "Properties": [
                {
                    "Property": {
                        "EoCloudCover": {
                            "LowerBound": 0,
                            "UpperBound": 20
                        }
                    }
                }
            ],
            "LogicalOperator": "AND"
        }
    }
}
eoj_config = {"LandCoverSegmentationConfig": {}}

response = sg_client.start_earth_observation_job(
    Name =  "seattle-landcover", 
    InputConfig = eoj_input_config,
    JobConfig = eoj_config, 
    ExecutionRoleArn = execution_role
)
my_eoj_arn = response['Arn']
```

After your EOJ is created, the `Arn` is returned to you\. You use the `Arn` to identify a job and perform further operations\. To get the status of a job, you can run `sg_client.get_earth_observation_job(Arn = response['Arn'])`\.

The following example shows how to query the status of an EOJ until it is completed\.

```
import time
import datetime

# check status of created Earth Observation Job and wait until it is completed
response = sg_client.get_earth_observation_job(Arn = my_eoj_arn)
while response['Status'] != 'COMPLETED':
    response = sg_client.get_earth_observation_job(Arn = my_eoj_arn)
    print("Earth Observation Job status: {} (Last update: {})".format(response['Status'], datetime.datetime.now()), end='\r')
    time.sleep(30)
```

After the EOJ is completed, you can visualize the EOJ outputs directly in the notebook\. The following example shows you how an interactive map can be rendered\.

```
map = sagemaker_geospatial_map.create_map({
'is_raster': True
})
map.set_sagemaker_geospatial_client(sg_client)
# render the map
map.render()
```

The following example shows how the map can be centered on an area of interest and the input and output of the EOJ can be rendered as separate layers within the map\.

```
# visualize the area of interest
config = {
'label': 'Seattle AoI'
}
aoi_layer = map.visualize_eoj_aoi(Arn = my_eoj_arn, config = config)
# visualize input
start_date = "2022-11-01T00:00:00Z"
end_date = "2022-11-22T23:59:59Z"
config = {
'label': 'Input'
}
input_layer = map.visualize_eoj_input(Arn = my_eoj_arn, config = config, time_range_filter={
"start_date": start_date,
"end_date": end_date
})
# visualize output, EOJ needs to be in completed status
start_date = "2022-11-01T00:00:00Z"
end_date = "2022-11-22T23:59:59Z"
config = {
'preset': 'singleBand',
'band_name': 'mask'
}
output_layer = map.visualize_eoj_output(Arn = my_eoj_arn, config = config, time_range_filter={
"start_date": start_date,
"end_date": end_date
})
```