# Amazon SageMaker Studio JumpStart<a name="studio-jumpstart"></a>

**Important**  
 To use new features with an existing notebook instance or Studio app, you must restart the notebook instance or the Studio app to get the latest updates\. 

 You can use Amazon SageMaker Studio JumpStart to learn about SageMaker features and capabilities through curated 1\-click solutions, example notebooks, and pretrained models that you can deploy\. You can also fine\-tune the models and deploy them\. 

 To access JumpStart, you must first launch SageMaker Studio\. JumpStart features are not available in SageMaker notebook instances, and you can't access them through SageMaker APIs or the AWS CLI\. 

 Open JumpStart by choosing the JumpStart icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-icon.png)\) in the *left sidebar*\. 

In the *file and resource browser* \(the left pane\), you can find JumpStart options\. From here you can choose to browse JumpStart for solutions, models, notebooks, and other resources, or you can view your currently launched solutions, endpoints, and training jobs\. 

 To see what JumpStart has to offer, choose the JumpStart icon, and then choose **Browse JumpStart**\. JumpStart opens in a new tab in the *main work area*\. Here you can browse 1\-click solutions, models, example notebooks, blogs, and video tutorials\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-assets.png) 

**Important**  
Amazon SageMaker JumpStart makes certain content available from third\-party sources\. This content may be subject to separate license terms\. You are responsible for reviewing and complying with any applicable license terms and making sure they are acceptable for your use case before downloading or using the content\. 

## Using JumpStart<a name="jumpstart-using"></a>

 At the top of the JumpStart page you can use search to look for topics of interest\.  

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-search.png) 

 You can find JumpStart resources by using search, or by browsing each category that follows the search panel: 
+  **Solutions** – Launch end\-to\-end machine learning solutions that tie SageMaker to other AWS services with one click\. 
+  **Text models** – Deploy and fine\-tune pretrained transformers for various natural language processing use cases\. 
+  **Vision models** – Deploy and fine\-tune pretrained models for image classification and object detection with one click\.
+  **SageMaker algorithms** – Train and deploy SageMaker built\-in Algorithms for various problem types with these example notebooks\. 
+  **Example notebooks** – Run example notebooks that use SageMaker features like spot instance training and experiments over a large variety of model types and use cases\. 
+  **Blogs** – Read deep dives and solutions from machine learning experts hosted by Amazon\. 
+  **Video tutorials** – Watch video tutorials for SageMaker features and machine learning use cases from machine learning experts hosted by Amazon\. 

## Solutions<a name="jumpstart-solutions"></a>

 When you choose a solution, JumpStart provides a description of the solution and a **Launch** button\. There are no configuration options\. Solutions launch all of the resources necessary to run the solution, including training and model hosting instances\. After JumpStart launches the solution, JumpStart provides a link to a notebook that you can use to explore the solution’s features\. You can delete all of the solution’s resources by choosing **Delete solution resources**\. 

## Models<a name="jumpstart-models"></a>

 Models are available for quick deployment directly from JumpStart\. You can also fine\-tune some of these models\. When you browse the models, you can scroll to the deploy and fine\-tune sections to the **Description** section\. In the **Description** section, you can learn more about the model, including what it can do with the model, what kind of inputs and outputs are expected, and the kind of data you need if you want to use transfer learning to fine\-tune the model\. 

## Deploy a model<a name="jumpstart-deploy"></a>

 When you deploy a model from JumpStart, SageMaker hosts the model and deploys an endpoint that you can use for inference\. JumpStart also provides an example notebook that you can use to access the model after it's deployed\. 

## Model Deployment Configuration<a name="jumpstart-config"></a>

 After you choose a model, the **Deploy Model** pane opens\. Choose **Deployment Configuration** to configure your model deployment\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy.png) 

 The default **Machine Type** for deploying a model depends on the model\. The machine type is the hardware that the training job runs on\. In the following example, the `ml.m5.large` instance is the default for this particular BERT model\. 

You can also change the **Endpoint Name**\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-config.png) 

## Fine\-Tune a Model<a name="jumpstart-fine-tune"></a>

 Fine\-tuning trains a pretrained model on a new dataset without training from scratch\. This process, also known as transfer learning, can produce accurate models with smaller datasets and less training time\. 

## Fine\-Tuning Data Source<a name="jumpstart-fine-tune-data"></a>

 When you fine\-tune a model, you can use the default dataset or choose your own data, which is located in an S3 bucket\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-fine-tune.png) 

 To browse the buckets available to you, choose **Find S3 bucket**\. These buckets are limited by the permissions used to set up your Studio account\. You can also specify an S3 URI by choosing **Enter S3 bucket location**\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-dataset.png) 

**Tip**  
 To find out how to format the data in your bucket, choose **Learn more**\. Also, the description section for the model has detailed information about inputs and outputs\.  

 For text models: 
+  The bucket must have a data\.csv file\. 
+  The first column must be a unique integer for the class label\. For example: 1, 2, 3, 4, n\. 
+  The second column must be a string\. 
+  The second column should have the corresponding text that matches the type and language for the model\.  

 For vision models: 
+  The bucket must have as many subdirectories as the number of classes\. 
+  Each subdirectory should contain images that belong to that class in \.jpg format\. 

**Note**  
 The S3 bucket must be in the same AWS Region where you're running SageMaker Studio because SageMaker doesn't allow cross\-region requests\. 

## Fine\-Tuning deployment configuration<a name="jumpstart-fine-tune-deploy"></a>

 The p3 family is recommended as the fastest for deep learning training, and this is recommended for fine\-tuning a model\. The following chart shows the number of GPUs in each instance type\. There are other available options that you can choose from, including p2 and g4 instance types\. 


|  |  | 
| --- |--- |
|  Instance type  |  GPUs  | 
|  p3\.2xlarge  |  1  | 
|  p3\.8xlarge  |  4  | 
|  p3\.16xlarge  |  8  | 
|  p3dn\.24xlarge  |  8  | 

## Hyperparameters<a name="jumpstart-hyperparameters"></a>

 You can customize the hyperparameters of the training job that are used to fine\-tune the model\.  

If you use the default dataset for text models without changing the hyperparameters, you get a nearly identical model as a result\. For vision models, the default dataset is different from the dataset used to train the pretrained models, so your model is different as a result\. 

 You have the following hyperparameter options: 
+ **Epochs** – One epoch is one cycle through the entire dataset\. Multiple intervals complete a batch, and multiple batches eventually complete an epoch\. Multiple epochs are run until the accuracy of the model reaches an acceptable level, or in other words, when the error rate drops below an acceptable level\. 
+ **Learning rate** – The amount that values should be changed between epochs\. As the model is refined, its internal weights are being nudged and error rates are checked to see if the model improves\. A typical learning rate is 0\.1 or 0\.01, where 0\.01 is a much smaller adjustment and could cause the training to take a long time to converge, whereas 0\.1 is much larger and can cause the training to overshoot\. It is one of the primary hyperparameters that you might adjust for training your model\. Note that for text models, a much smaller learning rate \(5e\-5 for BERT\) can result in a more accurate model\. 
+ **Batch size** – The number of records from the dataset that to be selected for each interval to send to the GPUs available in training\. In an image example, you might send out 32 images per GPU, so 32 would be your batch size\. If you choose an instance type with more than one GPU, the batch is divided by the number of GPUs\. Suggested batch size varies depending on the data and the model that you are using\. For example, how you optimize for image data differs from how you handle language data\. In the instance type chart in the deployment configuration section, you can see the number of GPUs per instance type\. Start with a standard recommended batch size \(for example, 32 for a vision model\)\. Then, multiply this by the number of GPUs in the instance type that you selected\. For example, if you're using a `p3.8xlarge`, this would be 32\(batch size\)\*4\(GPUs\), for a total of 128 as your batch size adjusted for the number of GPUs\. For a text model like BERT, try starting with a batch size of 64, and then reduce as needed\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-hyperparameters.png) 

## Training Output<a name="jumpstart-training"></a>

 When the fine\-tuning process is complete, JumpStart provides information about the model: parent model, training job name, training job Amazon Resource Name \(ARN\), training time, and output path\. The output path is where you can find your new model in an S3 bucket\. The folder structure uses the model name you provided and the model file is in an `/output` subfolder and it's always named `model.tar.gz`\.  

 Example: `s3://bucket/model-name/output/model.tar.gz` 