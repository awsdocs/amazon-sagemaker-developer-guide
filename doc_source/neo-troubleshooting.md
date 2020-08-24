# Troubleshooting Neo Compilation Errors<a name="neo-troubleshooting"></a>

This section contains information about how to understand and prevent common errors, the error messages they generate, and guidance on how to resolve these errors\. It also contains lists of the frameworks and the operations in each of those frameworks that Neo supports\. 

**Topics**
+ [Prevent Neo Input Errors](#neo-troubleshooting-errors-preventing)
+ [Neo Error Messages](#neo-troubleshooting-errors-understanding)
+ [Resolve Neo Errors](#neo-errors-resolving)

## Prevent Neo Input Errors<a name="neo-troubleshooting-errors-preventing"></a>

Some of the most common errors are due to invalid input formats\. 

***What input data shapes does Neo expect?***

Neo expects the name and shape of the expected data inputs for your trained model with JSON format or list format\. The data inputs are framework specific\. 
+ `TENSORFLOW`: Specify either the name and shape \(NHWC format\) of the expected data inputs using a dictionary format for your trained model\. The dictionary formats required are as follows:
  + For one input: `{'input':[1,1024,1024,3]}`
  + For two inputs: `{'data1': [1,28,28,1], 'data2':[1,28,28,1]}`
+ `KERAS`: Specify either the name and shape \(NCHW format\) of expected data inputs using a dictionary format for your trained model\. Note that while Keras model artifacts should be uploaded in NHWC \(channel\-last\) format, `DataInputConfig` should be specified in NCHW \(channel\-first\) format\. The dictionary formats required are as follows:
  + For one input: `{'input_1':[1,3,224,224]}`
  + For two inputs: `{'input_1': [1,3,224,224], 'input_2':[1,3,224,224]}`
+ `MXNET/ONNX`: Specify either the name and shape \(NCHW format\) of the expected data inputs in order using a dictionary format for your trained model\. The dictionary formats required are as follows:
  + For one input: `{'data':[1,3,1024,1024]}`
  + For two inputs: `{'var1': [1,1,28,28], 'var2':[1,1,28,28]}`
+ `PYTORCH`: Specify either the name and shape \(NCHW format\) of expected data inputs in order using a dictionary format for your trained model or you can specify the shape only using a list format\. The dictionary formats required are are as follows:
  + For one input in dictionary format: `{'input0':[1,3,224,224]}`
  + For one input in list format: `[[1,3,224,224]]`
  + For two inputs in dictionary format: `{'input0':[1,3,224,224], 'input1':[1,3,224,224]} `
  + For two inputs in list format: `[[1,3,224,224], [1,3,224,224]]`
+ `XGBOOST`: input data name and shape are not needed\.
+ `TFLITE`: Specify either the name and shape \(NHWC format\) of the expected data inputs in order using a dictionary format for your trained model\. The dictionary formats required are as follows:
  + For one input: `{'input':[1,224,224,3]}`

## Neo Error Messages<a name="neo-troubleshooting-errors-understanding"></a>

This section lists and classifies Neo errors and error messages\.

### Neo Error Messages<a name="neo-error-messages"></a>

This list catalogs the user and system error messages you can receive from Neo deployments\.
+ **User error messages**
  + **Client permission error**: Neo passes the errors for these straight through from the dependent service\.

    *Access Denied* when calling sts:AssumeRole

    Any *400 error* when calling S3 to download or upload a client model\.

    *PassRole* error
  + **Load error**: Keywords in error messages, 'InputConfiguration','ModelSizeTooBig'\.

    *Load Error: InputConfiguration*: Exactly one \{\.xxx\} file is allowed for \{yyy\} model\.

    *Load Error: ModelSizeTooBig*: number of nodes in a tree can't exceed 2^31
  + **Compilation error**: Keywords in error messages, 'OperatorNotImplemented',' OperatorAttributeNotImplemented', 'OperatorAttributeRequired', 'OperatorAttributeValueNotValid'\.

    *OperatorNotImplemented*: \{xxx\} is not supported\.

    *OperatorAttributeNotImplemented*: \{xxx\} is not supported in \{yyy\}\.

    *OperatorAttributeRequired*: Required attribute \{xxx\} not found in \{yyy\}\.

    *OperatorAttributeValueNotValid*: The value of attribute \{xxx\} in operator \{yyy\} cannot be negative\.
  + **Any Malformed Input Errors** 
+ **System error messages**
  + For system errors, Neo shows only one error message similar to the following: There was an unexpected error during compilation, check your inputs and try again in a few minutes\.
  + This covers all unexpected errors and errors that are not user errors\.

### Neo Error Classifications<a name="neo-errors"></a>

This list classifies the *user errors* you can receive from Neo\. These include access and permission errors and load errors for each of the supported frameworks\. All other errors are *system errors*\.
+ **Client permission error**: Neo passes the errors for these straight through from the dependent service\.

  *Access Denied* when calling sts:AssumeRole

  Any *400 error* when calling Amazon S3 to download or upload a client model\.

  *PassRole* error
+ **Load error**: Assuming that the Neo compiler successfully loaded \.tar\.gz from Amazon S3, check whether the tarball contains the necessary files for compilation\. The checking criteria is framework\-specific:
  + **TensorFlow**: Expects only protobuf file \(\*\.pb or \*\.pbtxt\)\. For *saved models*, expects one `variables` folder\.
  + **Pytorch**: Expect only one pytorch file \(\*\.pth\)\.
  + **MXNET**: Expect only one symbol file \(\*\.json\) and one parameter file \(\*\.params\)\.
  + **XGBoost**: Expect only one XGBoost model file \(\*\.model\)\. The input model has size limitation\.
+ **Compilation error**: Assuming that the Neo compiler successfully loaded \.tar\.gz from Amazon S3, and that the tarball contains necessary files for compilation\. The checking criteria is:
  + **OperatorNotImplemented**: An operator has not been implemented\.
  + **OperatorAttributeNotImplemented**: The attribute in the specified operator has not been implemented\.
  + **OperatorAttributeRequired**: An attribute is required for an internal symbol graph, but it is not listed in the user input model graph\.
  + **OperatorAttributeValueNotValid**: The value of the attribute in the specific operator is not valid\.

## Resolve Neo Errors<a name="neo-errors-resolving"></a>

This section provides guidance on troubleshooting common issues with Neo\. These include permission, load, compilation, and system errors and errors involving invalid inputs and unsupported operations\.
+ **Catalog of Known Issues**:
  + If you see **Client Permission Error**, review the set up documentation and make sure that you have correctly granted the permissions that are failing\.
  + If you see **Load Error**, check the model format files that Neo expects for different frameworks\.
  + If you see **Compilation Error**, check and address the details error message in your input model graph\.
  + If you see **System Error**, try again in a few minutes\. If that fails, file a ticket\.
+ **Lack of Roles and Permissions**: Review the set up documentation and make sure that you have correctly granted the permissions that are failing\.
+ **Invalid API and Console Inputs**: Fix your input as described in the validation error\.
+ **Unsupported Operators**: 
  + Check the failure reason where Neo has listed all unsupported operators with the keyword ‘OperatorNotImplemented’\.
  + For example: Compilation Error: OperatorNotImplemented: The following operators are not implemented: \{'\_sample\_multinomial', 'RNN' \}
  + Remove the unsupported operators from your input model graph and test it again\.