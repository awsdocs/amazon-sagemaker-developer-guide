# The Amazon SageMaker Programming Model<a name="how-it-works-prog-model"></a>

Amazon SageMaker provides APIs that you can use to create and manage notebook instances and train and deploy models\. For more information, see [API Reference](API_Reference.md)\. 

Making API calls directly from code is cumbersome, and requires you to write code to authenticate your requests\. Amazon SageMaker provides the following alternatives:
+ **Use the Amazon SageMaker console**—With the console, you don't write any code\. You use the console UI to start model training or deploy a model\. The console works well for simple jobs, where you use a built\-in training algorithm and you don't need to preprocess training data\. 

   
+ **Modify the example Jupyter notebooks**—Amazon SageMaker provides several Jupyter notebooks that train and deploy models using specific algorithms and datasets\. Start with a notebook that has a suitable algorithm and modify it to accommodate your data source and specific needs\.

   
+ **Write model training and inference code from scratch**—Amazon SageMaker provides both an AWS SDK and a high\-level Python library that you can use in your code to start model training jobs and deploy the resulting models\.

   
  + **The high\-level Python library**—The Python library simplifies model training and deployment\. In addition to authenticating your requests, the library abstracts platform specifics by providing simple methods and default parameters\. For example:

     
    + To deploy your model, you call only the `deploy()` method\. The method creates an Amazon SageMaker model, an endpoint configuration, and an endpoint\.

       
    + If you use a custom framework script for model training, you call the `fit()` method\. The method creates a \.gzip file of your script, uploads it to an Amazon S3 location, and then runs it for model training, and other tasks\. For more information, see [Using Machine Learning Frameworks with Amazon SageMaker](frameworks.md)\.

       
  + **The AWS SDK** —The SDKs provide methods that correspond to the Amazon SageMaker API \(see [Actions](API_Operations.md)\)\. Use the SDKs to programmatically start a model training job and host the model in Amazon SageMaker\. SDK clients authenticate your requests by using your access keys, so you don't need to write authentication code\. They are available in multiple languages and platforms\. For more information, see [SDKs](https://aws.amazon.com/tools/)\. 

     

  In [Getting Started](gs.md), you train and deploy a model using an algorithm provided by Amazon SageMaker\. That exercise shows how to use both of these libraries\. For more information, see [Getting Started](gs.md)\.

   
+ **Integrate Amazon SageMaker into your Apache Spark workflow**—Amazon SageMaker provides a library for calling its APIs from Apache Spark\. With it, you can use Amazon SageMaker\-based estimators in an Apache Spark pipeline\. For more information, see [Using Apache Spark with Amazon SageMaker](apache-spark.md)\.

## How It Works: Next Topic<a name="howitwork-prog-model-nextstep"></a>

 [Getting Started](gs.md) 