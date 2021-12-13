# Pricing<a name="inference-recommender-pricing"></a>

Amazon SageMaker Inference Recommender only charges you for the instances used while your jobs are executing\. The following examples use estimates based on pricing in the `us-east-2` Region\. Your costs may differ based on your Region and instance types\. For more information about pricing, see [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/)\.

## Instance Recommendation Pricing Example<a name="instance-pricing-example1"></a>

When you provide a model version in the model registry, Inference Recommender downloads sample data for that model version from Amazon S3 to local file storage and creates endpoints for up to 5 instance types for an Inference Recommender job with the `Default` job type\.

The typical job duration is roughly 45 minutes but can vary based on the machine learning model, inference script, and sample payloads provided\. Also, instance types selected by Inference Recommender can change based on the ML model\.


| Hours | Instance | Cost per hour | Total | 
| --- | --- | --- | --- | 
| 0\.75 | ml\.r5d\.large | 0\.173 | 0\.12975 | 
| 0\.75 | ml\.t2\.medium | 0\.056 | 0\.042 | 
| 0\.75 | ml\.c5d\.large | 0\.115 | 0\.08625 | 
| 0\.75 | ml\.m5\.large | 0\.115 | 0\.08625 | 
| 0\.75 | ml\.inf1\.xlarge | 0\.297 | 0\.22275 | 

The preceding table lists the cost per hour for five different instances in the `us-east-2` region\. The total price for this example would be $0\.57\.

## Load Test Recommendation Pricing Example<a name="load-test-pricing-example2"></a>

When you provide a model version in the model registry, Inference Recommender downloads sample data for that model version from Amazon S3 to local file storage and creates endpoints for each instance configuration provided for an Inference Recommender job with the `Advanced` job type\.

The typical job duration is roughly 2 hours\. Total cost is highly dependent on the traffic pattern and instance configurations provided\.


| Hours | Instance | Cost per hour | Total | 
| --- | --- | --- | --- | 
| 2 | ml\.r5d\.large | 0\.173 | 0\.346 | 
| 2 | ml\.t2\.medium | 0\.056 | 0\.112 | 
| 2 | ml\.c5d\.large | 0\.115 | 0\.23 | 
| 2 | ml\.m5\.large | 0\.115 | 0\.23 | 
| 2 | ml\.inf1\.xlarge | 0\.297 | 0\.594 | 

The table lists the cost per hour for five different instances in the `us-east-2` Region\. The total price for this example would be $1\.51\.