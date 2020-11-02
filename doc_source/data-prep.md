# Prepare and Label Data<a name="data-prep"></a>

To analyze your data and evaluate machine learning models on Amazon SageMaker, use [Amazon SageMaker Processing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)\. With Processing, you can use a simplified, managed experience on SageMaker to run your data processing workloads, such as feature engineering, data validation, model evaluation, and model interpretation\. You can also use the Amazon SageMaker Processing APIs during the experimentation phase and after the code is deployed in production to evaluate performance\.

To train a machine learning model, you need a large, high\-quality, labeled dataset\. You can label your data using Amazon SageMaker Ground Truth\. Choose from one of the Ground Truth [built\-in task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) or create your own [custom labeling workflow](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-custom-templates.html)\. To improve the accuracy of your data labels and reduce the total cost of labeling your data, use Ground Truth enhanced data labeling features like [automated data labeling](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html) and [annotation consolidation](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-annotation-consolidation.html)\. 

**Topics**
+ [Process Data and Evaluate Models](processing-job.md)
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Create and Manage Workforces](sms-workforce-management.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)