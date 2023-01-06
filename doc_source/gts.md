# Use Amazon SageMaker Ground Truth Synthetic Data to Generate and Label Data<a name="gts"></a>

Amazon SageMaker Ground Truth synthetic data is a turnkey data generation and labeling service that makes it quicker and more cost effective for machine learning \(ML\) scientists to acquire images that are used to train computer vision \(CV\) models\. To train a CV model, ML scientists need large, high\-quality, labeled datasets\. With Ground Truth synthetic data, ML scientists can generate and label thousands of images within days\. Ground Truth synthetic data uses computer\-generated 3D models to create virtual environments representing real\-world scenarios, generates synthetic images captured from these environments, and automatically annotates each image with labels\. You can use the labeled synthetic images with AWS’s CV model training services such as Amazon SageMaker and Amazon Lookout for Vision\.
<a name="why-use-gts"></a>
**Why use Ground Truth Synthetic Data?**  
Collecting and labeling data in dynamic environments with variations in object size, shape, color, position, background, and lighting is often a time\-consuming and expensive process\. To effectively train a model to operate in a dynamic environment, ML scientists must collect a large set of real\-world images to represent all possible scenarios, a process that can take months\. For scenarios that don’t occur frequently, such as rare product defects and faulty product placement, it can take years to capture a sufficient number of images to train a CV model\. To acquire images with product defects, ML scientists may intentionally damage products in order to acquire defective images\. Ground Truth synthetic data makes it faster and more cost effective for ML scientists to quickly acquire labeled images that represent real\-world scenarios, a core requirement for training CV models\. ML scientists can use Ground Truth synthetic data to generate thousands of synthetic images from 3D virtual environments representing real world scenarios in hours instead of months\. Ground Truth provides a synthetic image fidelity and diversity report and a manifest file along with the labeled synthetic data\. The synthetic image fidelity and diversity report provides statistics and plots that help you better understand the generated synthetic images\. The manifest file contains information about the images and image labels that you can use to train and test a model\.

**Note**  
 Ground Truth synthetic data does not support PHI, PCI, or FedRAMP certified data, and you should not provide this data to Ground Truth synthetic data\. 

Ground Truth synthetic data has the following functionalities\.
+ Full 3D scenes and multiple cameras in a scene
+ Ground truth depth maps that provide 3D depth data for all generated images
+ Sequences of images \(video\) from multiple synchronized cameras
+ A moving conveyor belt that supports dynamic scenes
<a name="how-do-i-use-gts"></a>
**How do I use Ground Truth Synthetic Data?**  
If you are a first\-time user of Ground Truth synthetic data, we recommend that you follow the procedures outlined in the [Getting Started with Amazon SageMaker Ground Truth Synthetic Data](gts-getting-started.md) section\.