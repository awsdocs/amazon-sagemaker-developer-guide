# Get Started with a SageMaker Clarify Container<a name="clarify-processing-job-configure-container"></a>

Amazon SageMaker provides prebuilt SageMaker Clarify container images that include the libraries and other dependencies needed to compute bias metrics and feature attributions for explainability\. This image has been enabled to run SageMaker [Process Data](processing-job.md) in your account\. 

The image URIs for the containers are in the following form:

```
<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/sagemaker-clarify-processing:1.0
```

For example:

```
205585389593.dkr.ecr.us-east-1.amazonaws.com/sagemaker-clarify-processing:1.0
```

The following table lists the addresses by AWS Region\.


**Docker Images for Clarify Processing Jobs**  

| Region | Image address | 
| --- | --- | 
| us\-east\-1 | 205585389593\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| us\-east\-2 | 211330385671\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| us\-west\-1 | 740489534195\.dkr\.ecr\.us\-west\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| us\-west\-2 | 306415355426\.dkr\.ecr\.us\-west\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-east\-1 | 098760798382\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-south\-1 | 452307495513\.dkr\.ecr\.ap\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-northeast\-1 | 377024640650\.dkr\.ecr\.ap\-northeast\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-northeast\-2 | 263625296855\.dkr\.ecr\.ap\-northeast\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-northeast\-3 | 912233562940\.dkr\.ecr\.ap\-northeast\-3\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-southeast\-1 | 834264404009\.dkr\.ecr\.ap\-southeast\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-southeast\-2 | 007051062584\.dkr\.ecr\.ap\-southeast\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ca\-central\-1 | 675030665977\.dkr\.ecr\.ca\-central\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-central\-1 | 017069133835\.dkr\.ecr\.eu\-central\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-west\-1 | 131013547314\.dkr\.ecr\.eu\-west\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-west\-2 | 440796970383\.dkr\.ecr\.eu\-west\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-west\-3 | 341593696636\.dkr\.ecr\.eu\-west\-3\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-north\-1 | 763603941244\.dkr\.ecr\.eu\-north\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| me\-south\-1 | 835444307964\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| sa\-east\-1 | 520018980103\.dkr\.ecr\.sa\-east\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| af\-south\-1 | 811711786498\.dkr\.ecr\.af\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-south\-1 | 638885417683\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| cn\-north\-1 | 122526803553\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn/sagemaker\-clarify\-processing:1\.0 | 
| cn\-northwest\-1 | 122578899357\.dkr\.ecr\.cn\-northwest\-1\.amazonaws\.com\.cn/sagemaker\-clarify\-processing:1\.0 | 