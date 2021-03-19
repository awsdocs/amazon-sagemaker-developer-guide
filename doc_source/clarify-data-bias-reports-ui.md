# Generate Reports for Bias in Pretraining Data in SageMaker Studio<a name="clarify-data-bias-reports-ui"></a>

SageMaker Clarify is integrated with Amazon SageMaker Data Wrangler, which can help you identify bias during data preparation without having to write your own code\. Data Wrangler provides an end\-to\-end solution to import, prepare, transform, featurize, and analyze data with Amazon SageMaker Studio\. For an overview of the Data Wrangler data prep workflow, see [Prepare ML Data with Amazon SageMaker Data Wrangler](data-wrangler.md)\. You specify attributes of interest, such as gender or age, and SageMaker Clarify runs a set of algorithms to detect the presence of bias in those attributes\. After the algorithm runs, SageMaker Clarify provides a visual report with a description of the sources and severity of possible bias so that you can plan steps to mitigate\. For example, in a financial dataset that contains few examples of business loans to one age group as compared to others, SageMaker flags the imbalance so that you can avoid a model that disfavors that age group\.

**To analyze and report on data bias**

To get started with Data Wrangler, see [Get Started with Data Wrangler](data-wrangler-getting-started.md)\.

1. Open Amazon SageMaker Studio and choose **Create Data Flow** from the **Import and prepare your data** tile\.  
![\[Create data flow in Data Wrangler.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-create-data-flow.PNG)

1. From the **Import data** tab, choose **Amazon S3** and then specify your data source on the **Data sources/S3 source** page\.  
![\[Import your data.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-import-data-s3.PNG)

1. After you have imported your data, choose the plus sign on the **Data flow** page and then choose **Add Analysis**\.  
![\[Add an analysis for the imported data.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-add-analysis.PNG)

1. On the **Create Analysis** page, go to the **Configure** panel and then choose **Bias Report** from the **Chart** menu\.  
![\[Description of the image.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-select-bias-report.PNG)

1. Configure the bias report by providing the **Name**, the column to predict and whether it is a value or threshold, the column to analyze for bias \(the facet\) and whether it is a value or threshold\.  
![\[Configure the bias report 1.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-configure-bias-report.PNG)

1. Continue configuring the bias report by choosing the bias metrics\.  
![\[Choose the bias metric.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-configure-choose-bias-metrics.PNG)

1. Choose **Check for bias** to generate and view the bias report\. Scroll down to view all of the reports\.  
![\[Generate and view the bias report.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-generate-bias-report.PNG)

1. Choose the carrot to the right of the bias metric description to see documentation that can help you interpret the significance of the metric values\.  
![\[Help interpreting the data bias metrics.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-read-bias-report.PNG)

1. To view a table summary of the bias metric values, choose the table, You can save the report for export by choosing **Create** in the lower\-right corner of the page\.  
![\[View a table summary of the bias metric values save the report.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-save-bias-report.PNG)

1. On the page where your data bias reports are stored, choose the **Export** tab to download the reports\.  
![\[The data bias report.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-data-wrangler-export-bias-report.PNG)