# SageMaker geospatial capabilities<a name="geospatial-encryption-at-rest"></a>

You can protect your data at rest using encryption for SageMaker geospatial\.
<a name="geospatial-encryption-at-rest-gmk"></a>
**Server\-Side Encryption with Amazon SageMaker geospatial managed key \(Default\)**  
Amazon SageMaker geospatial capabilities encrypts all your data, including computational results from your `EarthObservationJobs` and `VectorEnrichmentJobs` along with all your service metadata\. There is no data that is stored within Amazon SageMaker unencrypted\. It uses a default AWS managed key to encrypt all your data\. 
<a name="geospatial-encryption-at-rest-ksm"></a>
**Server\-Side Encryption with KMS Keys Stored in AWS Key Management Service \(SSE\-KMS\)**  
Currently, Amazon SageMaker geospatial capabilities do not support encryption using a customer\-owned KMS key\.