# Recommendation jobs<a name="inference-recommender-recommendation-jobs"></a>

Amazon SageMaker Inference Recommender can make two types of recommendations:

1. Instance recommendations \(`Default` job type\) run a set of load tests on recommended instance types\. You only need to provide a model package Amazon Resource Name \(ARN\) to launch this type of recommendation job\. Instance recommendation jobs complete within 45 minutes\.

1. Endpoint recommendations \(`Advanced` job type\) are based on a custom load test where you select ML instances, provide a custom traffic pattern, and provide requirements for latency and throughput based on your production requirements\. This job takes an average of 2 hours to complete depending on the job duration set and the total number of instance configurations tested\.

Both types of recommendations use the same APIs to create, describe, and stop jobs\. The output is a list of instance configuration recommendations with associated environment variables, cost, throughput, and latency metrics\. Endpoint recommendations also provide an initial instance count which you can use to configure an autoscaling policy\. To differentiate between the two types of jobs, specify `Default` to create preliminary endpoint recommendations and `Advanced` for custom load testing and endpoint recommendations\.

**Note**  
You do not need to do both types of recommendation jobs in your own workflow\. You can do either independently of each other\.

**Topics**
+ [Get an instance recommendation](inference-recommender-instance-recommendation.md)
+ [Get compiled recommendations with Neo](inference-recommender-neo-compilation.md)
+ [Run a custom load test](inference-recommender-load-test.md)