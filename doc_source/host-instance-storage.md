# Hosting Instance Storage Volumes<a name="host-instance-storage"></a>

When you create an endpoint, Amazon SageMaker attaches an Amazon EBS storage volume to each ML compute instance that hosts the endpoint\. The size of the storage volume depends on the instance type\. The following table shows the size of the storage volume that Amazon SageMaker attaches for each instance type\.


| Instance Type | Storage Volume in GB | 
| --- | --- | 
| ml\.t2\.medium | 2 | 
| ml\.t2\.large | 4 | 
| ml\.t2\.xlarge | 8 | 
| ml\.t2\.2xlarge | 16 | 
| ml\.m4\.xlarge | 8 | 
| ml\.m4\.2xlarge | 16 | 
| ml\.m4\.4xlarge | 30 | 
| ml\.m4\.10xlarge | 30 | 
| ml\.m4\.16xlarge | 30 | 
| ml\.m5\.large | 4 | 
| ml\.m5\.xlarge | 8 | 
| ml\.m5\.2xlarge | 16 | 
| ml\.m5\.4xlarge | 30 | 
| ml\.m5\.12xlarge | 30 | 
| ml\.m5\.24xlarge | 30 | 
| ml\.c4\.large | 4 | 
| ml\.c4\.xlarge | 4 | 
| ml\.c4\.2xlarge | 8 | 
| ml\.c4\.4xlarge | 15 | 
| ml\.c4\.8xlarge | 30 | 
| ml\.c5\.large | 2 | 
| ml\.c5\.xlarge | 4 | 
| ml\.c5\.2xlarge | 8 | 
| ml\.c5\.4xlarge | 16 | 
| ml\.c5\.9xlarge | 30 | 
| ml\.c5\.18xlarge | 30 | 
| ml\.p2\.xlarge | 30 | 
| ml\.p2\.8xlarge | 30 | 
| ml\.p2\.16xlarge | 30 | 
| ml\.p3\.2xlarge | 30 | 
| ml\.p3\.8xlarge | 30 | 
| ml\.p3\.16xlarge | 30 | 