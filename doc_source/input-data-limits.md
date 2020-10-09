# Input Data Quotas<a name="input-data-limits"></a>

Input datasets used in semantic segmentation labeling jobs have a quota of 20,000 items\. For all other labeling job types, the dataset size quota is 100,000 items\. To request an increase to the quota for labeling jobs other than semantic segmentation jobs, review the procedures in [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) to request a quota increase\.

Input image data for active and non\-active learning labeling jobs must not exceed size and resolution quotas\. *Active learning* refers to labeling job that use [automated data labeling](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html)\. *Non\-active learning* refers to labeling jobs that don't use automated data labeling\.

Additional quotas apply for label categories for all task types, and for input data and labeling category attributes for 3D point cloud and video frame task types\. 

## Input File Size Quota<a name="input-file-size-limit"></a>

Input files can't exceed the following size\- quotas for both active and non\-active learning labeling jobs\.


| Labeling Job Task Type | Input File Size Quota | 
| --- | --- | 
| Image classification | 40 MB | 
| Bounding box \(Object detection\) | 40 MB | 
| Semantic segmentation | 40 MB | 
| Image classification label adjustment | 40 MB | 
| Bounding box \(Object detection\) label adjustment | 40 MB | 
| Semantic segmentation label adjustment | 40 MB | 
| Image classification label verification | 40 MB | 
| Bounding box \(Object detection\) label verification | 40 MB | 
| Semantic segmentation label verification | 40 MB | 

## Input Image Resolution Quotas<a name="non-active-learning-input-data-limits"></a>

Image file resolution refers to the number of pixels in an image, and determines the amount of detail an image holds\. Image resolution quotas differ depending on the labeling job type and the SageMaker built\-in algorithm used\. The following table lists the resolution quotas for images used in active and non\-active learning labeling jobs\.


| Labeling Job Task Type | **Resolution Quota \- Non Active Learning** | Resolution Quota \- Active Learning | 
| --- | --- | --- | 
| Image classification | 100 million pixels | 3840 x 2160 pixels \(4 K\) | 
| Bounding box \(Object detection\) | 100 million pixels | 3840 x 2160 pixels \(4 K\) | 
| Semantic segmentation | 100 million pixels | 1920 x 1080 pixels \(1080 p\) | 
| Image classification label adjustment | 100 million pixels | 3840 x 2160 pixels \(4 K\) | 
| Object detection label adjustment | 100 million pixels | 3840 x 2160 pixels \(4 K\) | 
| Semantic segmentation label adjustment | 100 million pixels | 1920 x 1080 pixels \(1080 p\) | 
| Image classification label verification | 100 million pixels | Not available | 
| Object detection label verification | 100 million pixels | Not available | 
| Semantic segmentation label verification | 100 million pixels | Not available | 

## Label Category Quotas<a name="sms-label-quotas"></a>

Each labeling job task type has a quota for the number of label categories you can specify\. Workers select label categories to create annotations\. For example, you may specify label categories *car*, *pedestrian*, and *biker* when creating a bounding box labeling job and workers will select the *car* category before drawing bounding boxes around cars\.

**Important**  
Label category names cannot exceed 256 characters\.   
All label categories must be unique\. You cannot specify duplicate label categories\. 

The following label category limits apply to labeling jobs\. Quotas for label categories depend on whether you use the SageMaker API operation `CreateLabelingJob` or the console to create a labeling job\.


****  

| Labeling Job Task Type | Label Category Quota \- API | Label Category Quota \- Console | 
| --- | --- | --- | 
| Image classification \(Multi\-label\) | 50 | 50 | 
| Image classification \(Single label\) | Unlimited | 30 | 
| Bounding box \(Object detection\) | 50 | 50 | 
| Label verification | Unlimited | 30 | 
| Semantic segmentation \(with active learning\) | 20 | 10 | 
| Semantic segmentation \(without active learning\) | Unlimited | 10 | 
| Named entity recognition | Unlimited | 30 | 
| Text classification \(Multi\-label\) | 50 | 50 | 
| Text classification \(Single label\) | Unlimited | 30 | 
| Video classification | 30 | 30 | 
| Video frame object detection | 30 | 30 | 
| Video frame object tracking | 30 | 30 | 
| 3D point cloud object detection | 30 | 30 | 
| 3D point cloud object tracking | 30 | 30 | 
| 3D point cloud semantic segmentation | 30 | 30 | 

## 3D Point Cloud and Video Frame Labeling Job Quotas<a name="sms-input-data-quotas-other"></a>

The following quotas apply for 3D point cloud and video frame labeling job input data\. 


****  

| Labeling Job Task Type | Input Data Quota | 
| --- | --- | 
| Video frame object detection  |  2,000 video frames \(images\) per sequence  | 
| Video frame object detection  |  10 video frame sequences per manifest file | 
| Video frame object tracking |  2,000 video frames \(images\) per sequence  | 
| Video frame object tracking |  10 video frame sequences per manifest file | 
| 3D point cloud object detection |  100,000 point cloud frames per labeling job | 
| 3D point cloud object tracking |  100,000 point cloud frame sequences per labeling job | 
| 3D point cloud object tracking |  500 point cloud frames in each sequence file | 

When you create a video frame or 3D point cloud labeling job, you can add one or more *label category attributes* to each label category that you specify to have workers provide more information about an annotation\.

Each label category attribute has a single label category attribute `name`, and a list of one or more options \(values\) to choose from\. To learn more, see [Worker User Interface \(UI\)](sms-point-cloud-general-information.md#sms-point-cloud-worker-task-ui) for 3D point cloud labeling jobs and [Worker User Interface \(UI\)](sms-video-overview.md#sms-video-worker-task-ui) for video frame labeling jobs\. 

 The following quotas apply to the number of label category attributes names and values you can specify for labeling jobs\.


****  

| Labeling Job Task Type | Label Category Attribute \(name\) Quota | Label Category Attribute Values Quota | 
| --- | --- | --- | 
| Video frame object detection  | 10 | 10 | 
| Video frame object tracking | 10 | 10 | 
| 3D point cloud object detection | 10 | 10 | 
| 3D point cloud object tracking | 10 | 10 | 
| 3D point cloud semantic segmentation | 10 | 10 | 