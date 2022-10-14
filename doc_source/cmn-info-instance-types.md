# Instance Types for Built\-in Algorithms<a name="cmn-info-instance-types"></a>

For training and hosting Amazon SageMaker algorithms, we recommend using the following Amazon EC2 instance types:
+  ml\.m5\.xlarge, ml\.m5\.4xlarge, and ml\.m5\.12xlarge
+  ml\.c5\.xlarge, ml\.c5\.2xlarge, and ml\.c5\.8xlarge 
+  ml\.p3\.xlarge, ml\.p3\.8xlarge, and ml\.p3\.16xlarge 

Most Amazon SageMaker algorithms have been engineered to take advantage of GPU computing for training\. For most algorithm training, we support P2, P3, G4dn, and G5 GPU instances\. Despite higher per\-instance costs, GPUs train more quickly, making them more cost effective\. Exceptions are noted in this guide\.

The size and type of data can have a great effect on which hardware configuration is most effective\. When the same model is trained on a recurring basis, initial testing across a spectrum of instance types can discover configurations that are more cost\-effective in the long run\. Additionally, algorithms that train most efficiently on GPUs might not require GPUs for efficient inference\. Experiment to determine the most cost effectiveness solution\.

For more information on SageMaker hardware specifications, see [Amazon SageMaker ML Instance Types](https://aws.amazon.com/sagemaker/pricing/instance-types/)\.