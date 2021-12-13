# Use Amazon SageMaker Ground Truth Plus to Label Data<a name="gtp"></a>

Amazon SageMaker Ground Truth Plus is a turnkey data labeling service that uses an expert workforce to deliver high\-quality annotations quickly and reduces costs by up to 40%\. Using Ground Truth Plus, data scientists and business managers, such as data operations managers and program managers, can create high\-quality training datasets without having to build labeling applications and manage labeling workforces on their own\. You can get started with Amazon SageMaker Ground Truth Plus by uploading data along with the labeling requirements in Amazon S3\. 
<a name="why-use-gtp"></a>
**Why use Ground Truth Plus?**  
To train a machine learning \(ML\) model, data scientists need large, high\-quality, labeled datasets\. As ML adoption grows, labeling needs increase\. This forces data scientists to spend weeks on building data labeling workflows and managing a data labeling workforce\. Unfortunately, this slows down innovation and increases cost\. To ensure data scientists can spend their time building, training, and deploying ML models, data scientists typically task other in\-house teams consisting of data operations managers and program managers to produce high\-quality training datasets\. However, these teams typically don't have access to skills required to deliver high\-quality training datasets, which affects ML results\. As a result, you look for a data labeling partner that can help them create high\-quality training datasets at scale without consuming their in\-house resources\.

When you upload the data, Ground Truth Plus sets up the data labeling workflows and operates them on your behalf\. From there, an expert workforce trained on a varierty of machine learning \(ML\) tasks performs data labeling\. Ground Truth Plus currently offers two types of expert workforce: an Amazon employed workforce and a curated list of third\-party vendors\. Ground Truth Plus provides you with the flexibility to choose the labeling workforce\. AWS experts select the best labeling workforce based on your project requirements\. For example, if you need people proficient in labeling audio files, specify that in the guidelines provided to Ground Truth Plus, and the service automatically selects labelers with those skills\. 

**Note**  
 Ground Truth Plus does not support PHI, PCI or FedRAMP certified data, and you should not provide this data to Ground Truth Plus\. 
<a name="how-it-works-gtp"></a>
**How does it work?**  
There are five main components to the Ground Truth Plus workflow\.
+ Requesting a pilot
+ Sharing the data to be labeled
+ Creating a project team
+ Accessing the project portal to monitor progress of training datasets and review labeled data
+ Receiving the labeled data

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gtb-how-it-works.png)
<a name="how-do-i-use-gtp"></a>
**How do I use Ground Truth Plus?**  
If you are a first\-time user of Ground Truth Plus, we recommend that you follow the procedures outlined in the [Getting Started with Amazon SageMaker Ground Truth Plus\.](gtp-getting-started.md) section\.