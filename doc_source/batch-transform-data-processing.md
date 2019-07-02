# Associate Prediction Results with their Corresponding Input Records<a name="batch-transform-data-processing"></a>

When making predictions on a large dataset, attributes that are not needed for prediction can be excluded\. After the predictions have been made, you often want to associate some of the excluded attributes with those predictions or with other input data in your report\. Amazon SageMaker Batch Tranform enables these data processing steps, often eliminating the need for any additional pre\-processing or post\-processing\. The feature supports JSON and CSV formatted input files\. 

**Topics**
+ [Data Processing Workflow for a Batch Transform Job](#batch-transform-data-processing-workflow)
+ [Use Data Processing in Batch Transform Jobs](#batch-transform-data-processing-steps)
+ [Supported JSONPath Operators](#data-processing-operators)
+ [Examples](#batch-transform-data-processing-examples)

## Data Processing Workflow for a Batch Transform Job<a name="batch-transform-data-processing-workflow"></a>

The following diagram shows the data processing workflow for a batch transform job\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/batch-transform-data-processing.png)

To join the prediction results with the input data, there are three main steps:
+ Filter the input data that is not needed for inference before passing it to the batch transform job\. Use the [InputFilter](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-Type-DataProcessing-InputFilter) parameter to determine which attributes to use as input for the model\.
+ Associate the input data with the inference results\. Use [JoinSource](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-Type-DataProcessing-JoinSource) to combine the input data with the inference\.
+ Filter the joined data to retain the inputs needed to provide context for interpreting the predictions in the reports\. Use [OutputFilter](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-Type-DataProcessing-OutputFilter) to store the specified portion of the joined dataset in the output file\.

## Use Data Processing in Batch Transform Jobs<a name="batch-transform-data-processing-steps"></a>

To process the data when creating a batch transform job with [CreateTransformJob](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html):

1. Specify the portion of the input to pass to the model with the `InputFilter` parameter in the `DataProcessing` data structure\. 

1. Join the raw input data with the transformed data with the `JoinSource` parameter\.

1. Specify which portion of the joined input and transformed data from the batch transform job to include in the output file with the `OutputFilter` parameter\.

1.  Choose either JSON\- or CSV\-formatted files for input: 
   + For JSON\- or JSON Lines\-formatted inputs, Amazon SageMaker either adds `SageMakerOutput` attribute to the input file or creates a new JSON output file with the attributes `SageMakerInput` and `SageMakerOutput`\. For more information, see [DataProcessing](API_DataProcessing.md)\. 
   + For CSV\-formatted input files, the joined input data is followed by the transformed data and the output is a CSV file\.

If you use an algorithm with the DataProcessing structure, it must support your chosen format for *both* input and output files\. For example, with the [TransformOutput](API_TransformOutput.md) field of the `CreateTransformJob` API, you must set both the [ContentType](https://docs.aws.amazon.com/sagemaker/latest/dg/API_TransformInput.html#SageMaker-Type-TransformInput-ContentType) and [Accept](https://docs.aws.amazon.com/sagemaker/latest/dg/API_TransformOutput.html#SageMaker-Type-TransformOutput-Accept) parameters to one of the following values: `text/csv`, `application/json`, or `application/jsonlines`\. The syntax for specifying columns in a CSV file and specifying attributes in a JSON file are different\. Using the wrong syntax causes an error\. For more information, see [Examples](#batch-transform-data-processing-examples) For more information about input and output file formats for build\-in algorithms, see [Use Amazon SageMaker Built\-in Algorithms ](algos.md)\.

The record delimiters for the input and output must also be consistent with the your chosen file input\. The [SplitType](https://docs.aws.amazon.com/sagemaker/latest/dg/API_TransformInput.html#SageMaker-Type-TransformInput-SplitType) parameter indicates how to split the records in the input dataset\. The [AssembleWith](https://docs.aws.amazon.com/sagemaker/latest/dg/API_TransformOutput.html#SageMaker-Type-TransformOutput-AssembleWith) parameter indicates how to reassemble the records for the output\. If you set input and output formats to `text/csv`, you must also set the `SplitType` and `AssemblyType` parameters to `line`\. If you set the input and output formats to `application/jsonlines`, you can set both `SplitType` and `AssemblyType` to either `none` or `line`\.

 For JSON files, the attribute name `SageMakerOutput` is reserved for output\. The JSON input file can't have an attribute with this name\. If it does, the data in the input file might be overwritten\. 

## Supported JSONPath Operators<a name="data-processing-operators"></a>

To filter and join the input data and inference, use a JSONPath subexpression\. The following table lists the supported JSONPath operators\.


| JSONPath Operator | Description | Example | 
| --- | --- | --- | 
| $ |  The root element to a query\. This operator is required at the beginning of all path expressions\.  | "$" | 
| \.<name> |  A dot\-notated child element\.  |  `"$.id"`  | 
| \* |  A wildcard\. Use in place of an attribute name or numeric value\.  |  `"$.id.*"`  | 
| \['<name>' \(,'<name>'\)\] |  A bracket\-notated element or multiple child elements\.  |  `"$['id','SageMakerOutput']"`  | 
| \[<number> \(,<number>\)\] |  An index or array of indexes\. Negative index values are also supported\. A `-1` index refers to the last element in an array\.  |  `$[1]` , `$[1,3,5]`  | 
| \[<start>:<end>\] |  An array slice operator\. If you omit *<start>*, Amazon SageMaker uses the first element of the array\. If you omit *<end>*, Amazon SageMaker uses the last element of the array\.  |  `$[2:5]`, `$[:5]`, `$[2:]`  | 

**Note**  
Amazon SageMaker supports only a subset of the defined JSONPath operators\. For more information about JSONPath operators, see [JsonPath](https://github.com/json-path/JsonPath)\.

## Examples<a name="batch-transform-data-processing-examples"></a>

The following examples show some common ways to join input data with prediction results\.

**Topics**
+ [Output Only Inference Results](#batch-transform-data-processing-example-default)
+ [Output a Combination of Input Data and Results](#batch-transform-data-processing-example-all)
+ [Output an ID Column with Results and Exclude the ID Column from the Input \(CSV\)](#batch-transform-data-processing-example-select-csv)
+ [Output an ID Attribute with Results and Exclude the ID Attribute from the Input \(JSON\)](#batch-transform-data-processing-example-select-json)

### Output Only Inference Results<a name="batch-transform-data-processing-example-default"></a>

By default, the [DataProcessing](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-DataProcessing) parameter doesn't join results with input and only outputs the inference results\.

If you want to explicity specify in code not to join results with input, use the Amazon SageMaker Python SDK and specify these settings in a transformer call\.

```
sm_transformer = sagemaker.transformer.Transformer(…)
sm_transformer.transform(…, input_filter="$", join_source= "None", output_filter="$")
```

The following code shows the default behavior\. To output an inference only using the AWS SDK for Python, add it to your CreateTransformJob request:

```
{
    "DataProcessing": {
        "InputFilter": "$",
        "JoinSource": "None",
        "OutputFilter": "$"
    }
}
```

### Output a Combination of Input Data and Results<a name="batch-transform-data-processing-example-all"></a>

If you are using the Amazon SageMaker Python SDK, combine the input data with the inference in the output file, specify `"Input"` for the [JoinSource](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-Type-DataProcessing-JoinSource) parameter in a transformer call\.

```
sm_transformer = sagemaker.transformer.Transformer(…)
sm_transformer.transform(…, join_source= "Input")
```

If you are using the AWS SDK for Python \(Boto 3\), join all input data with the inference by adding the following code to your [CreateTransformJob](API_CreateTransformJob.md) request\.

```
{
    "DataProcessing":
    {
        "JoinSource": "Input"
    }
}
```

For JSON or JSON Lines, the results are in the `SageMakerOutput` key in the input JSON file\. For example, if the input is a JSON file that contains the key\-value pair `{"key":1}`, the the data transform result might be `{"label":1}`\.

Amazon SageMaker stores both in the input file under `SageMakerInput` key\.

```
{
    "key":1,
    "SageMakerOutput":{"label":1}
}
```

**Note**  
The joined result for JSON must be a key\-value pair object\. If the input is not a key\-value pair object, Amazon SageMaker creates a new JSON file\. In the new JSON file, the input data is stored in the `SageMakerInput` key and the results are stored as the `SageMakerOutput` value\.

For a CSV file, for example, if the record is `[1,2,3]`, and the label result is `[1]`, then the output file would contain `[1,2,3,1]`\.

### Output an ID Column with Results and Exclude the ID Column from the Input \(CSV\)<a name="batch-transform-data-processing-example-select-csv"></a>

If you are using the Amazon SageMaker Python SDK, to include results or an ID column in the output, specify indexes of the joined dataset in a transformer call\. For example, if your data includes five columns an the first one is the ID column, use the following transformer request\.

```
sm_transformer = sagemaker.transformer.Transformer(…)
sm_transformer.transform(…, input_filter="$[1:]", join_source= "Input", output_filter="$[0,5:]")
```

If you are using the AWS SDK for Python \(Boto 3\), add the following code to your [CreateTransformJob](API_CreateTransformJob.md) request\.

```
{
    "DataProcessing": {
        "InputFilter": "$[1:]",
        "JoinSource": "Input",
        "OutputFilter": "$[0,5:]"
    }
}
```

To specify columns in Amazon SageMaker, index the array elements\. The first column is 0, the second column is 1, and the sixth column is 5\. To exclude the first column from the input, set [InputFilter](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-Type-DataProcessing-InputFilter) to `"$[1:]"`\. 

The [OutputFilter](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-Type-DataProcessing-OutputFilter) parameter applies to the joined input and output\. To index correctly, you must know the sizes of the input data combined with the inference\. To include the first three columns from the input data with the output, set the `OutputFilter` to `"$[0:2, 5:]"`\. The colon `‘:’` tells Amazon SageMaker to include all of the elements between two values\. For example, 0:2 specifies the first three columns\. If you omit the number after the colon, for example, `"[0, 5:]"`, the subset ends at the last column in the joined data\.

### Output an ID Attribute with Results and Exclude the ID Attribute from the Input \(JSON\)<a name="batch-transform-data-processing-example-select-json"></a>

If you are using the Amazon SageMaker Python SDK, include results or an ID attribute in the output by specifying it in a transformer call\. For example, if you store data under the `features` attribute and the record ID under the `ID` attribute, you would use the following transformer request\.

```
sm_transformer = sagemaker.transformer.Transformer(…)
sm_transformer.transform(…, input_filter="$.features", join_source= "Input", output_filter="$['id','SageMakerOutput']")
```

If you are using the AWS SDK for Python \(Boto 3\), join all input data with the inference by adding the following code to your [CreateTransformJob](API_CreateTransformJob.md) request\.

```
{
    "DataProcessing": {
        "InputFilter": "$.features",
        "JoinSource": "Input",
        "OutputFilter": "$['id','SageMakerOutput']"
    }
}
```

**Warning**  
The attribute name `SageMakerOutput` is reserved for the JSON output file\. The JSON input file must not have an attribute with this name\. If it does, the input file values might be overwritten with the inference\.