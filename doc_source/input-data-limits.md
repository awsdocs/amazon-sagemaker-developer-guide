# Input Data Quotas<a name="input-data-limits"></a>

Input datasets used in semantic segmentation labeling jobs have a quota of 20,000 items\. For all other labeling job types, the dataset size quota is 100,000 items\. To request an increase to the quota for labeling jobs other than semantic segmentation jobs, review the procedures in [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) to request a quota increase\.

Input image data for active and non\-active learning labeling jobs must not exceed size and resolution quotas\. *Active learning* refers to labeling job that use [automated data labeling](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html)\. *Non\-active learning* refers to labeling jobs that don't use automated data labeling\.

## Input File Size Quota<a name="input-file-size-limit"></a>

Input files can't exceed the following size\- quotas for both active and non\-active learning labeling jobs\.


| Labeling Job | Input File Quotas | 
| --- | --- | 
| Image classification | 40 MB | 
| Object detection \(bounding box\)  | 40 MB | 
| Semantic segmentation | 40 MB | 
| Image classification label adjustment | 40 MB | 
| Object detection label adjustment | 40 MB | 
| Semantic segmentation label adjustment | 40 MB | 
| Image classification label verification | 40 MB | 
| Object detection label verification | 40 MB | 
| Semantic segmentation label verification | 40 MB | 

## Input Image Resolution Quotas<a name="non-active-learning-input-data-limits"></a>

Image file resolution refers to the number of pixels in an image, and determines the amount of detail an image holds\. Image resolution quotas differ depending on the labeling job type and the SageMaker built\-in algorithm used\. The following table lists the resolution quotas for images used in active and non\-active learning labeling jobs\.


| Labeling Job | **Resolution Quota \- Non Active Learning** | Resolution Quota \- Active Learning | 
| --- | --- | --- | 
| Image classification | 100 million pixels | 3840 x 2160 pixels \(4 K\) | 
| Object detection \(bounding box\) | 100 million pixels | 3840 x 2160 pixels \(4 K\) | 
| Semantic segmentation | 100 million pixels | 1920 x 1080 pixels \(1080 p\) | 
| Image classification label adjustment | 100 million pixels | 3840 x 2160 pixels \(4 K\) | 
| Object detection label adjustment | 100 million pixels | 3840 x 2160 pixels \(4 K\) | 
| Semantic segmentation label adjustment | 100 million pixels | 1920 x 1080 pixels \(1080 p\) | 
| Image classification label verification | 100 million pixels | Not available | 
| Object detection label verification | 100 million pixels | Not available | 
| Semantic segmentation label verification | 100 million pixels | Not available | 

## 3D Point Cloud and Video Frame Labeling Job Quotas<a name="sms-input-data-quotas-other"></a>

The following quotas apply for 3D point cloud and video frame labeling jobs\. 


****  

| Labeling Job | Quota | 
| --- | --- | 
| Video frame object detection  |  2,000 video frames \(images\) per sequence  | 
| Video frame object detection  |  10 video frame sequences per manifest file | 
| Video frame object tracking |  2,000 video frames \(images\) per sequence  | 
| Video frame object tracking |  10 video frame sequences per manifest file | 
| 3D point cloud object detection |  100,000 point cloud frames per labeling job | 
| 3D point cloud object tracking |  100,000 point cloud frame sequences per labeling job | 
| 3D point cloud object tracking |  500 point cloud frames in each sequence file | 