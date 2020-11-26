# Load testing your autoscaling configuration<a name="endpoint-scaling-loadtest"></a>

Perform load tests to choose an automatic scaling configuration that works the way you want\.

For an example of load testing to optimize automatic scaling for an Amazon SageMaker endpoint, see [Load test and optimize an Amazon SageMaker endpoint using automatic scaling](https://aws.amazon.com/blogs/machine-learning/load-test-and-optimize-an-amazon-sagemaker-endpoint-using-automatic-scaling/)\.

The following guidelines for load testing assume you are using an automatic scaling policy that uses the predefined target metric `SageMakerVariantInvocationsPerInstance`\.

**Topics**
+ [Determine the performance characteristics](#endpoint-scaling-loadtest-variant)
+ [Calculate the target load](#endpoint-scaling-loadtest-calc)

## Determine the performance characteristics<a name="endpoint-scaling-loadtest-variant"></a>

Perform load testing to find the peak `InvocationsPerInstance` that your model's production variant can handle, and the latency of requests, as concurrency increases\.

This value depends on the instance type chosen, payloads that clients of your model typically send, and the performance of any external dependencies your model has\.

**To find the peak requests\-per\-second \(RPS\) your model's production variant can handle and latency of requests**

1. Set up an endpoint with your model using a single instance\. For information about how to set up an endpoint, see [Step 6\.1: Deploy the Model to SageMaker Hosting Services](ex1-deploy-model.md)\.

1. Use a load testing tool to generate an increasing number of parallel requests, and monitor the RPS and model latency in the out put of the load testing tool\. 
**Note**  
You can also monitor requests\-per\-minute instead of RPS\. In that case don't multiply by 60 in the equation to calculate `SageMakerVariantInvocationsPerInstance` shown below\.

   When the model latency increases or the proportion of successful transactions decreases, this is the peak RPS that your model can handle\.

## Calculate the target load<a name="endpoint-scaling-loadtest-calc"></a>

After you find the performance characteristics of the variant, you can determine the maximum RPS we should allow to be sent to an instance\. The threshold used for scaling must be less than this maximum value\. Use the following equation in combination with load testing to determine the correct value for the `SageMakerVariantInvocationsPerInstance` target metric in your automatic scaling configuration\.

```
SageMakerVariantInvocationsPerInstance = (MAX_RPS * SAFETY_FACTOR) * 60
```

Where `MAX_RPS` is the maximum RPS that you determined previously, and `SAFETY_FACTOR` is the safety factor that you chose to ensure that your clients don't exceed the maximum RPS\. Multiply by 60 to convert from RPS to invocations\-per\-minute to match the per\-minute CloudWatch metric that SageMaker uses to implement automatic scaling \(you don't need to do this if you measured requests\-per\-minute instead of requests\-per\-second\)\.

**Note**  
SageMaker recommends that you start testing with a `SAFETY_FACTOR` of 0\.5\. Test your automatic scaling configuration to ensure it operates in the way you expect with your model for both increasing and decreasing customer traffic on your endpoint\.