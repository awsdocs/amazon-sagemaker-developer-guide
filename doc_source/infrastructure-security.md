# Infrastructure Security in Amazon SageMaker<a name="infrastructure-security"></a>

As a managed service, Amazon SageMaker is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access Amazon SageMaker through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

**Topics**
+ [Connect a Notebook Instance to Resources in a VPC](appendix-notebook-and-internet-access.md)
+ [Training and Inference Containers Run in Internet\-Free Mode](mkt-algo-model-internet-free.md)
+ [SageMaker Scans AWS Marketplace Training and Inference Containers for Security Vulnerabilities](#mkt-container-scan)
+ [Connect to SageMaker Through a VPC Interface Endpoint](interface-vpc-endpoint.md)
+ [Give SageMaker Processing Jobs Access to Resources in Your Amazon VPC](process-vpc.md)
+ [Give SageMaker Training Jobs Access to Resources in Your Amazon VPC](train-vpc.md)
+ [Give SageMaker Hosted Endpoints Access to Resources in Your Amazon VPC](host-vpc.md)
+ [Give Batch Transform Jobs Access to Resources in Your Amazon VPC](batch-vpc.md)

## SageMaker Scans AWS Marketplace Training and Inference Containers for Security Vulnerabilities<a name="mkt-container-scan"></a>

To meet our security requirements, algorithms and model packages listed in AWS Marketplace are scanned for Common Vulnerabilities and Exposures \(CVE\)\. CVE is a list of publicly known information about security vulnerability and exposure\. The National Vulnerability Database \(NVD\) provides CVE details such as severity, impact rating, and fix information\. Both CVE and NVD are available for public consumption and free for security tools and services to use\. For more information, see [http://cve\.mitre\.org/about/faqs\.html\#what\_is\_cve](http://cve.mitre.org/about/faqs.html#what_is_cve)\. 