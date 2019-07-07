# Step 4: Download, Explore, and Transform the Training Data<a name="ex1-preprocess-data"></a>

Download the MNIST dataset to your notebook instance, review the data, transform it, and upload it to your S3 bucket\. 

You transform the data by changing its format from `numpy.array` to comma\-separated values \(CSV\)\. The [XGBoost Algorithm](xgboost.md) expects input in either the LIBSVM or CSV format\. LIBSVM is an open source machine learning library\. In this exercise , you use CSV format because it's simpler\.

**Topics**
+ [Step 4\.1: Download the MNIST Dataset](ex1-preprocess-data-pull-data.md)
+ [Step 4\.2: Explore the Training Dataset](ex1-preprocess-data-inspect.md)
+ [Step 4\.3: Transform the Training Dataset and Upload It to Amazon S3](ex1-preprocess-data-transform.md)