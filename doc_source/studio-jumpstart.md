# SageMaker JumpStart<a name="studio-jumpstart"></a>

SageMaker JumpStart provides pretrained, open\-source models for a wide range of problem types to help you get started with machine learning\. You can incrementally train and tune these models before deployment\. JumpStart also provides solution templates that set up infrastructure for common use cases, and executable example notebooks for machine learning with SageMaker\.

You can access the pretrained models, solution templates, and examples through the JumpStart landing page in Amazon SageMaker Studio\. The following steps show how to access JumpStart models and solutions using Amazon SageMaker Studio\.

You can also access JumpStart models using the SageMaker Python SDK\. For information about how to use JumpStart models programmatically, see [Use SageMaker JumpStart Algorithms with Pretrained Models](https://sagemaker.readthedocs.io/en/stable/overview.html#use-sagemaker-jumpstart-algorithms-with-pretrained-models)\.

## Open and use JumpStart<a name="jumpstart-open-use"></a>

The following sections give information on how to open, use, and manage JumpStart from the Amazon SageMaker Studio UI\.

### Open JumpStart<a name="jumpstart-open"></a>

In Amazon SageMaker Studio, open the JumpStart landing page either through the **Home** page or the **Home** menu on the left\-side panel\. 
+ From the **Home** page you can either:
  + Choose **Quick start solutions** in the **Prebuilt and automated solutions** pane\. This opens the **SageMaker JumpStart** landing page\.
  + Choose a model directly in the **Quick start solutions** pane, or choose **Browse all Quick start solutions**\. This opens the **SageMaker JumpStart** landing page\.
+ From the **Home** menu in the left panel you can either:
  + Navigate to the **Quick start solutions** node, then choose **Solutions, models, example notebooks**\. This opens the **SageMaker JumpStart** landing page\.
  + Navigate to the **Quick start solutions** node, then choose **Launched Quick start assets**\.

    The **Launch quick start assets** page lists your currently launched solutions, deployed model endpoints, and training jobs created with Quick start\. You can access the JumpStart landing page from this tab by clicking on the **Browse Quick start solutions** button at the top right of the tab\.

The JumpStart landing page lists available end\-to\-end machine learning solutions, pretrained models, and example notebooks\. From any individual solution or model page, you can choose the **Browse JumpStart** button \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-browse-button.png)\) at the top right of the tab to return to the **SageMaker JumpStart** page\.

![\[SageMaker Studio interface with access to JumpStart from the Home navigation menu and the Home page.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-assets.png)

**Important**  
Before downloading or using third\-party content: You are responsible for reviewing and complying with any applicable license terms and making sure that they are acceptable for your use case\. 

### Use JumpStart<a name="jumpstart-using"></a>

From the **SageMaker JumpStart** landing page, you can browse for solutions, models, notebooks, and other resources\.

![\[SageMaker Studio JumpStart landing page.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-use.png)

You can find JumpStart resources by using the search bar, or by browsing each category\. Use the tabs to filter the available solutions by categories:
+  **Solutions** – In one step, launch comprehensive machine learning solutions that tie SageMaker to other AWS services\. Select **Explore All Solutions** to view all available solutions\.
+  **ML tasks** – Find a model by problem type \(e\.g\., Image Classification, Image Embedding, Object Detection, Text Generation\)\. Select **Explore All Models** to view all available models\.
+  **Data types** – Find a model by data type \(e\.g\., Vision, Text, Tabular, Audio\)\. Select **Explore All Models** to view all available models\.
+  **Notebooks** – Find example notebooks that use SageMaker features across multiple model types and use cases\. Select **Explore All Notebooks** to view all available example notebooks\.
+  **Frameworks** – Find a model by framework \(e\.g\., PyTorch, TensorFlow, Hugging Face\)\.
+  **Resources** – Use example notebooks, blogs, and video tutorials to learn and head start your problem types\.
  +  **Blogs** – Read details and solutions from machine learning experts\. 
  +  **Video tutorials** – Watch video tutorials for SageMaker features and machine learning use cases from machine learning experts\.
  +  **Example notebooks** – Run example notebooks that use SageMaker features like Spot Instance training and experiments over a large variety of model types and use cases\. 

### Manage JumpStart<a name="jumpstart-managing"></a>

From the **Home** menu in the left panel, navigate to **Quick start solutions**, then choose **Launched Quick start assets** to list your currently launched solutions, deployed model endpoints, and training jobs created with Quick start\.

![\[SageMaker Studio JumpStart Launched Quick start assets page.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-manage.png)

**Topics**
+ [Open and use JumpStart](#jumpstart-open-use)
+ [Solution Templates](jumpstart-solutions.md)
+ [Models](jumpstart-models.md)
+ [Shared Models and Notebooks](jumpstart-content-sharing.md)
+ [Amazon SageMaker JumpStart Industry: Financial](studio-jumpstart-industry.md)