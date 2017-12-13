# Step 3\.2: Download, Explore, and Transform the Training Data<a name="ex1-preprocess-data"></a>

Now download the MNIST dataset to your notebook instance\. Then review the data, transform it, and upload it to your S3 bucket\. 

You transform the data by changing its format from `numpy.array` to `RecordIO`\. The `RecordIO` format is more efficient for the algorithms provided by Amazon SageMaker\. For information about the `RecordIO` format, see [Data Format](http://mxnet.io/architecture/note_data_loading.html#data-format)\. 


+ [Step 3\.2\.1: Download the MNIST Dataset](ex1-preprocess-data-pull-data.md)
+ [Step 3\.2\.2: Explore the Training Dataset](ex1-preprocess-data-inspect.md)
+ [Step 3\.2\.3: Transform the Training Dataset and Upload It to S3](ex1-preprocess-data-transform.md)