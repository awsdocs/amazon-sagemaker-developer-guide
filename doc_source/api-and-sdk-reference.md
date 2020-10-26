# API Reference Guide for Amazon SageMaker<a name="api-and-sdk-reference"></a>

## Overview<a name="api-and-sdk-reference-overview"></a>

Amazon SageMaker provides APIs, SDKs, and a command line interface that you can use to create and manage notebook instances and train and deploy models\. 
+ [https://sagemaker.readthedocs.io/en/stable/](https://sagemaker.readthedocs.io/en/stable/) — Recommended\!
+ [Amazon SageMaker API Reference](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Reference.html)
+ [Amazon Augmented AI API Reference](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)
+ [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/index.html#cli-aws-sagemaker)
+ [AWS SDK for \.NET](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/SageMaker/NSageMaker.html)
+ [AWS SDK for C\+\+](https://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_sage_maker.html)
+ [AWS SDK for Go](https://docs.aws.amazon.com/sdk-for-go/api/service/sagemaker/)
+ [AWS SDK for Java](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sagemaker/AmazonSageMaker.html)
+ [AWS SDK for JavaScript](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SageMaker.html)
+ [AWS SDK for PHP](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sagemaker-2017-07-24.html)
+ [AWS SDK for Python \(Boto\)](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html)
+ [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/SageMaker.html)
+ [Amazon SageMaker Spark](https://github.com/aws/sagemaker-spark/blob/master/README.md)

You can also get code examples from the Amazon SageMaker example notebooks GitHub repository\.
+ [Example notebooks](https://github.com/awslabs/amazon-sagemaker-examples)

## Programming Model for Amazon SageMaker<a name="how-it-works-prog-model"></a>

Making API calls directly from code is cumbersome, and requires you to write code to authenticate your requests\. Amazon SageMaker provides the following alternatives:
+ **Use the Amazon SageMaker console**–With the console, you don't write any code\. You use the console UI to start model training or deploy a model\. The console works well for simple jobs, where you use a built\-in training algorithm and you don't need to preprocess training data\. 

   
+ **Modify the example Jupyter notebooks**–Amazon SageMaker provides several Jupyter notebooks that train and deploy models using specific algorithms and datasets\. Start with a notebook that has a suitable algorithm and modify it to accommodate your data source and specific needs\.

   
+ **Write model training and inference code from scratch**–Amazon SageMaker provides multiple AWS SDK languages \(listed in the overview\) and the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), a high\-level Python library that you can use in your code to start model training jobs and deploy the resulting models\.

   
  + **The Amazon SageMaker Python SDK**–This Python library simplifies model training and deployment\. In addition to authenticating your requests, the library abstracts platform specifics by providing simple methods and default parameters\. For example:

     
    + To deploy your model, you call only the `deploy()` method\. The method creates an Amazon SageMaker model artifact, an endpoint configuration, then deploys the model on an endpoint\.

       
    + If you use a custom framework script for model training, you call the `fit()` method\. The method creates a \.gzip file of your script, uploads it to an Amazon S3 location, and then runs it for model training, and other tasks\. For more information, see [Use Machine Learning Frameworks, Python, and R with Amazon SageMaker](frameworks.md)\.

       
  + **The AWS SDKs** – The SDKs provide methods that correspond to the Amazon SageMaker API \(see [ `Operations`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Operations.html)\)\. Use the SDKs to programmatically start a model training job and host the model in Amazon SageMaker\. SDK clients authenticate your requests by using your access keys, so you don't need to write authentication code\. They are available in multiple languages and platforms\. For more information, see the preceeding list in the overview\. 

     

  In [Get Started with Amazon SageMaker](gs.md), you train and deploy a model using an algorithm provided by Amazon SageMaker\. That exercise shows how to use both of these libraries\. For more information, see [Get Started with Amazon SageMaker](gs.md)\.

   
+ **Integrate Amazon SageMaker into your Apache Spark workflow**–Amazon SageMaker provides a library for calling its APIs from Apache Spark\. With it, you can use Amazon SageMaker\-based estimators in an Apache Spark pipeline\. For more information, see [Use Apache Spark with Amazon SageMaker](apache-spark.md)\.