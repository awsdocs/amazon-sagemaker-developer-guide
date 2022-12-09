# Amazon SageMaker geospatial Map SDK<a name="geospatial-notebook-sdk"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

You can use Amazon SageMaker geospatial capabilities to visualize maps within the SageMaker geospatial UI as well as SageMaker notebooks with a geospatial image\. These visualizations are supported by the map visualization library called [Foursquare Studio](https://studio.foursquare.com/home)

You can use the APIs provided by the SageMaker geospatial map SDK to visualize your geospatial data, including the input, output, and AoI for EOJ\.

**Topics**
+ [add\_dataset API](#geo-add-dataset)
+ [update\_dataset API](#geo-update-dataset)
+ [add\_layer API](#geo-add-layer)
+ [update\_layer API](#geo-update-layer)
+ [visualize\_eoj\_aoi API](#geo-visualize-eoj-aoi)
+ [visualize\_eoj\_input API](#geo-visualize-eoj-input)
+ [visualize\_eoj\_output API](#geo-visualize-eoj-output)

## add\_dataset API<a name="geo-add-dataset"></a>

Adds a raster or vector dataset object to the map\.

**Request syntax**

```
Request = 
    add_dataset(
      self,
      dataset: Union[Dataset, Dict, None] = None,
      *,
      auto_create_layers: bool = True,
      center_map: bool = True,
      **kwargs: Any,
    ) -> Optional[Dataset]
```

**Request parameters**

The request accepts the following parameters\.

Positional arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
| `dataset` | Union\[Dataset, Dict, None\] | Data used to create a dataset, in CSV, JSON, or GeoJSON format \(for local datasets\) or a UUID string\. | 

Keyword arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
| `auto_create_layers` | Boolean | Whether to attempt to create new layers when adding a dataset\. Default value is `False`\. | 
| `center_map` | Boolean | Whether to center the map on the created dataset\. Default value is `True`\. | 
| `id` | String | Unique identifier of the dataset\. If you do not provide it, a random ID is generated\. | 
| `label` | String | Dataset label which is displayed\. | 
| `color` | Tuple\[float, float, float\] | Color label of the dataset\. | 
| `metadata` | Dictionary | Object containing tileset metadata \(for tiled datasets\)\. | 

**Response**

This API returns the [Dataset](https://location.foursquare.com/developer/docs/studio-map-sdk-types#dataset) object that was added to the map\.

## update\_dataset API<a name="geo-update-dataset"></a>

Updates an existing dataset's settings\.

**Request syntax**

```
Request = 
    update_dataset(
    self,
    dataset_id: str,
    values: Union[_DatasetUpdateProps, dict, None] = None,
    **kwargs: Any,
) -> Dataset
```

**Request parameters**

The request accepts the following parameters\.

Positional arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
| `dataset_id` | String | The identifier of the dataset to be updated\. | 
| `values` | Union\[[\_DatasetUpdateProps](https://location.foursquare.com/developer/docs/studio-map-sdk-types#datasetupdateprops), dict, None\] | The values to update\. | 

Keyword arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
| `label` | String | Dataset label which is displayed\. | 
| `color` | [RGBColor](https://location.foursquare.com/developer/docs/studio-map-sdk-types#rgbcolor) | Color label of the dataset\. | 

**Response**

This API returns the updated dataset object for interactive maps, or `None` for non\-interactive HTML environments\. 

## add\_layer API<a name="geo-add-layer"></a>

Adds a new layer to the map\. This function requires at least one valid layer configuration\.

**Request syntax**

```
Request = 
    add_layer(
    self,
    layer: Union[LayerCreationProps, dict, None] = None,
    **kwargs: Any
) -> Layer
```

**Request parameters**

The request accepts the following parameters\.

Arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
| `layer` | Union\[[LayerCreationProps](https://location.foursquare.com/developer/docs/studio-map-sdk-types#layercreationprops), dict, None\] | A set of properties used to create a layer\. | 

**Response**

The layer object that was added to the map\.

## update\_layer API<a name="geo-update-layer"></a>

Update an existing layer with given values\.

**Request syntax**

```
Request = 
    update_layer(
  self,
  layer_id: str,
  values: Union[LayerUpdateProps, dict, None],
  **kwargs: Any
) -> Layer
```

**Request parameters**

The request accepts the following parameters\.

Arguments


| Positional argument |  Type  |  Description  | 
| --- | --- | --- | 
| `layer_id` | String | The ID of the layer to be updated\. | 
| `values` | Union\[[LayerUpdateProps](https://location.foursquare.com/developer/docs/studio-map-sdk-types#layerupdateprops), dict, None\] | The values to update\. | 

Keyword arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
| `type` | [LayerType](https://location.foursquare.com/developer/docs/studio-map-sdk-types#layertype) | The type of layer\. | 
| `data_id` | String | Unique identifier of the dataset this layer visualizes\. | 
| `fields` | Dict \[string, Optional\[string\]\] | Dictionary that maps fields that the layer requires for visualization to appropriate dataset fields\. | 
| `label` | String | Canonical label of this layer\. | 
| `is_visible` | Boolean | Whether the layer is visible or not\. | 
| `config` | [LayerConfig](https://location.foursquare.com/developer/docs/studio-map-sdk-types#layerconfig) | Layer configuration specific to its type\.  | 

**Response**

Returns the updated layer object\.

## visualize\_eoj\_aoi API<a name="geo-visualize-eoj-aoi"></a>

Visualize the AoI of the given job ARN\.

**Request parameters**

The request accepts the following parameters\.

Arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
|  `Arn`  |  String  |  The ARN of the job\.  | 
|  `config`  |  Dictionary config = \{ label: <string> custom label of the added AoI layer, default AoI \}  |  An option to pass layer properties\.  | 

**Response**

Reference of the added input layer object\.

## visualize\_eoj\_input API<a name="geo-visualize-eoj-input"></a>

Visualize the input of the given EOJ ARN\.

**Request parameters**

The request accepts the following parameters\.

Arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
| `Arn` | String | The ARN of the job\. | 
| `time_range_filter` |  Dictionary time\_range\_filter = \{ start\_date: <string> date in ISO format end\_date: <string> date in ISO format \}  | An option to provide the start and end time\. Defaults to the raster data collection search start and end date\. | 
| `config` |  Dictionary config = \{ label: <string> custom label of the added output layer, default Input \}  | An option to pass layer properties\. | 

**Response**

Reference of the added input layer object\.

## visualize\_eoj\_output API<a name="geo-visualize-eoj-output"></a>

Visualize the output of the given EOJ ARN\.

**Request parameters**

The request accepts the following parameters\.

Arguments


| Argument |  Type  |  Description  | 
| --- | --- | --- | 
|  `Arn`  |  String  |  The ARN of the job\.  | 
|  `time_range_filter`  |  Dictionary time\_range\_filter = \{ start\_date: <string> date in ISO format end\_date: <string> date in ISO format \}  | An option to provide the start and end time\. Defaults to the raster data collection search start and end date\. | 
| `config` |  Dictionary config = \{ label: <string> custom label of the added output layer, default Output preset: <string> singleBand or trueColor, band\_name: <string>, only required for 'singleBand' preset\. Allowed bands for a EOJ \}  | An option to pass layer properties\. | 

**Response**

Reference of the added output Layer object\.

To learn more about visualizing your geospatial data, refer to [Visualization Using Amazon SageMaker geospatial](https://docs.aws.amazon.com/sagemaker/latest/dg/geospatial-visualize.html)\.