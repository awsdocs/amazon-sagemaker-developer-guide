# Host Instance Storage Volumes<a name="host-instance-storage"></a>

When you create an endpoint, Amazon SageMaker attaches an Amazon Elastic Block Store \(Amazon EBS\) storage volume to each ML compute instance that hosts the endpoint\. The size of the storage volume depends on the instance type\. 

The following table shows the size of the storage volume that Amazon SageMaker attaches for each instance type for a single endpoint and for a multi\-model endpoint\. For a MultiModel\-enabled container, the storage volume provisioned for its instances has more memory\. This allows more models to be cached on the instance storage volume\. MultiModel containers with GPU instance types \(for example, P2, P3, and G4 instance families\) aren't  supported on multi\-model endpoints\.

**Note**  
Because d instance types come with an NVMe SSD storage, Amazon SageMaker doesn't attach an Amazon EBS storage volume to these ML compute instances that host the multi\-model endpoint\.


| Instance Type | Storage Volume in GB | Storage Volume  for Multi\-Model Endpoint in GB | 
| --- | --- | --- | 
| ml\.t2\.medium | 2 | 8 | 
| ml\.t2\.large | 4 | 16 | 
| ml\.t2\.xlarge | 8 | 32 | 
| ml\.t2\.2xlarge | 16 | 64 | 
| ml\.m4\.xlarge | 8 | 32 | 
| ml\.m4\.2xlarge | 16 | 64 | 
| ml\.m4\.4xlarge | 30 | 128 | 
| ml\.m4\.10xlarge | 30 | 320 | 
| ml\.m4\.16xlarge | 30 | 512 | 
| ml\.m5\.large | 4 | 16 | 
| ml\.m5\.xlarge | 8 | 32 | 
| ml\.m5\.2xlarge | 16 | 64 | 
| ml\.m5\.4xlarge | 30 | 128 | 
| ml\.m5\.12xlarge | 30 | 384 | 
| ml\.m5\.24xlarge | 30 | 768 | 
| ml\.c4\.large | 4 | 7\.5 | 
| ml\.c4\.xlarge | 4 | 15 | 
| ml\.c4\.2xlarge | 8 | 30 | 
| ml\.c4\.4xlarge | 15 | 60 | 
| ml\.c4\.8xlarge | 30 | 120 | 
| ml\.c5\.large | 2 | 8 | 
| ml\.c5\.xlarge | 4 | 16 | 
| ml\.c5\.2xlarge | 8 | 32 | 
| ml\.c5\.4xlarge | 16 | 72 | 
| ml\.c5\.9xlarge | 30 | 144 | 
| ml\.c5\.18xlarge | 30 | 288 | 
| ml\.p2\.xlarge | 30 | Not supported | 
| ml\.p2\.8xlarge | 30 | Not supported | 
| ml\.p2\.16xlarge | 30 | Not supported | 
| ml\.p3\.2xlarge | 30 | Not supported | 
| ml\.p3\.8xlarge | 30 | Not supported | 
| ml\.p3\.16xlarge | 30 | Not supported | 
| ml\.r5\.large | 8 | 32 | 
| ml\.r5\.xlarge | 16 | 64 | 
| ml\.r5\.2xlarge | 30 | 128 | 
| ml\.r5\.4xlarge | 30 | 256 | 
| ml\.r5\.12xlarge | 30 | 762 | 
| ml\.r5\.24xlarge | 30 | 1536 | 