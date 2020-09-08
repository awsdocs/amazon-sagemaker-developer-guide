# Programming Model for Amazon SageMaker<a name="how-it-works-prog-model"></a>

Making API calls directly from code is cumbersome, and requires you to write code to authenticate your requests\. Amazon SageMaker provides the following alternatives:
+ **Use the SageMaker console**–With the console, you don't write any code\. You use the console UI to start model training or deploy a model\. The console works well for simple jobs, where you use a built\-in training algorithm and you don't need to preprocess training data\. 

   
+ **Modify the example Jupyter notebooks**–SageMaker provides several Jupyter notebooks that train and deploy models using specific algorithms and datasets\. Start with a notebook that has a suitable algorithm and modify it to accommodate your data source and specific needs\.

   
+ **Write model training and inference code from scratch**–SageMaker provides multiple AWS SDK languages \(listed in the overview\) and the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), a high\-level Python library that you can use in your code to start model training jobs and deploy the resulting models\.

   
  + **The SageMaker Python SDK**–This Python library simplifies model training and deployment\. In addition to authenticating your requests, the library abstracts platform specifics by providing simple methods and default parameters\. For example:

     
    + To deploy your model, you call only the `deploy()` method\. The method creates a SageMaker model artifact, an endpoint configuration, then deploys the model on an endpoint\.

       
    + If you use a custom framework script for model training, you call the `fit()` method\. The method creates a \.gzip file of your script, uploads it to an Amazon S3 location, and then runs it for model training, and other tasks\. For more information, see [Use Machine Learning Frameworks, Python, and R with Amazon SageMaker](frameworks.md)\.

       
  + **The AWS SDKs** – The SDKs provide methods that correspond to the SageMaker API \(see [ `Operations`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Operations.html)\)\. Use the SDKs to programmatically start a model training job and host the model in SageMaker\. SDK clients authenticate your requests by using your access keys, so you don't need to write authentication code\. They are available in multiple languages and platforms\. For more information, see the preceeding list in the overview\. 

     

  In [Get Started with Amazon SageMaker](gs.md), you train and deploy a model using an algorithm provided by SageMaker\. That exercise shows how to use both of these libraries\. For more information, see [Get Started with Amazon SageMaker](gs.md)\.

   
+ **Integrate SageMaker into your Apache Spark workflow**–SageMaker provides a library for calling its APIs from Apache Spark\. With it, you can use SageMaker\-based estimators in an Apache Spark pipeline\. For more information, see [Use Apache Spark with Amazon SageMaker](apache-spark.md)\.