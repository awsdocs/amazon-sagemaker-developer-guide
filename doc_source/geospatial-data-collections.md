# Data Collections<a name="geospatial-data-collections"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

Amazon SageMaker geospatial provides the following data collections to create an EOJ\.
+ [ USGS Landsat](https://www.usgs.gov/centers/eros/data-citation?qt-science_support_page_related_con=0#qt-science_support_page_related_con)
+ [Sentinel\-2](https://sentinel.esa.int/web/sentinel/missions/sentinel-2)

The image band information for these data collections is provided below\.


**USGS Landsat**  

| Band name | Wave length range \(nm\) | Units | Valid range | Fill value | Spatial resolution | 
| --- | --- | --- | --- | --- | --- | 
| coastal | 435 \- 451 | Unitless | 1 \- 65455 | 0 \(No Data\) | 30m | 
| blue | 452 \- 512 | Unitless | 1 \- 65455 | 0 \(No Data\) | 30m | 
| green | 533 \- 590 | Unitless | 1 \- 65455 | 0 \(No Data\) | 30m | 
| red | 636 \- 673 | Unitless | 1 \- 65455 | 0 \(No Data\) | 30m | 
| nir | 851 \- 879 | Unitless | 1 \- 65455 | 0 \(No Data\) | 30m | 
|  |  | Unitless | 1 \- 65455 | 0 \(No Data\) | 30m | 
| swir16 | 1566 \- 1651 | Unitless | 1 \- 65455 | 0 \(No Data\) | 30m | 
| swir22 | 2107 \- 2294 | Unitless | 1 \- 65455 | 0 \(No Data\) | 30m | 
| qa\_aerosol | NA | Bit Index | 0 \- 255 | 1 | 30m | 
| qa\_pixel | NA | Bit Index | 1 \- 65455 | 1 \(bit 0\) | 30m | 
| qa\_radsat | NA | Bit Index | 1 \- 65455 | NA | 30m | 
| t | 10600 \- 11190 | Scaled Kelvin | 1 \- 65455 | 0 \(No Data\) | 30m \(scaled from 100m\) | 
| atran | NA | Unitless | 0 \- 10000 | \-9999 \(No Data\) | 30m | 
| cdist | NA | Kilometers | 0 \- 24000 | \-9999 \(No Data\) | 30m | 
| drad | NA | W/\(m^2 sr µm\)/DN | 0 \- 28000 | \-9999 \(No Data\) | 30m | 
| urad | NA | W/\(m^2 sr µm\)/DN | 0 \- 28000 | \-9999 \(No Data\) | 30m | 
| trad | NA | W/\(m^2 sr µm\)/DN | 0 \- 28000 | \-9999 \(No Data\) | 30m | 
| emis | NA | Emissivity coefficient | 1 \- 10000 | \-9999 \(No Data\) | 30m | 
| emsd | NA | Emissivity coefficient | 1 \- 10000 | \-9999 \(No Data\) | 30m | 


**Sentinel\-2**  

| Band name | Wave length range \(nm\) | Scale | Valid range | Fill value | Spatial resolution | 
| --- | --- | --- | --- | --- | --- | 
| coastal | 443 | 0\.0001 | NA | 0 \(No Data\) | 60m | 
| blue | 490 | 0\.0001 | NA | 0 \(No Data\) | 10m | 
| green | 560 | 0\.0001 | NA | 0 \(No Data\) | 10m | 
| red | 665 | 0\.0001 | NA | 0 \(No Data\) | 10m | 
| rededge1 | 705 | 0\.0001 | NA | 0 \(No Data\) | 20m | 
| rededge2 | 740 | 0\.0001 | NA | 0 \(No Data\) | 20m | 
| rededge3 | 783 | 0\.0001 | NA | 0 \(No Data\) | 20m | 
| nir | 842 | 0\.0001 | NA | 0 \(No Data\) | 10m | 
| nir08 | 865 | 0\.0001 | NA | 0 \(No Data\) | 20m | 
| nir08 | 865 | 0\.0001 | NA | 0 \(No Data\) | 20m | 
| nir09 | 940 | 0\.0001 | NA | 0 \(No Data\) | 60m | 
| swir16 | 1610 | 0\.0001 | NA | 0 \(No Data\) | 20m | 
| swir22 | 2190 | 0\.0001 | NA | 0 \(No Data\) | 20m | 
| aot | Aerosol Optical Thickness | 0\.001 | NA | 0 \(No Data\) | 10m | 
| wvp | Scene\-average Water Vapour | 0\.001 | NA | 0 \(No Data\) | 10m | 
| scl | Scene classification data | NA | 1 \- 11 | 0 \(No Data\) | 20m | 