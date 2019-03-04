# Troubleshooting Neo Compilation Errors<a name="neo-troubleshooting"></a>

This section contains information about how to understand and prevent common errors, the error messages they generate, and guidance on how to resolve these errors\. It also contains lists of the frameworks and the operations in each of those frameworks that Neo supports\. 

**Topics**
+ [Prevent Neo Input Errors](#neo-troubleshooting-errors-preventing)
+ [Neo Error Messages](#neo-troubleshooting-errors-understanding)
+ [Resolve Neo Errors](#neo-errors-resolving)

## Prevent Neo Input Errors<a name="neo-troubleshooting-errors-preventing"></a>

Some of the most common errors are due to invalid inputs\. This section contains information arranged in question and answer form to help you avoid these errors\.

***Which frameworks does Neo support?***
+ [TensorFlow](https://aws.amazon.com/tensorflow/)
+ [PyTorch](https://pytorch.org/)
+ [Apache MXNET](https://aws.amazon.com/mxnet/)
+ [XGBoost](https://github.com/dmlc/xgboost)
+ [ONNX](https://github.com/onnx/onnx)

***Which operators does Amazon SageMaker Neo support for these frameworks?***

The following table lists the supported operations for each framework\.


| MXNet | TensorFlow | PyTorch/ONNX | 
| --- | --- | --- | 
| '\_add\_scalar' | 'Add' | 'Abs' | 
| '\_add\_symbol' | 'ArgMax' | 'Add' | 
| '\_contrib\_MultiBoxDetection' | 'ArgMin' | 'ArgMax' | 
| '\_contrib\_MultiBoxPrior' | 'AvgPool' | 'ArgMin' | 
| '\_copy' | 'BatchNormWithGlobalNormalization'  | 'AveragePool' | 
| '\_div\_scalar' | 'BiasAdd' | 'BatchNormalization' | 
| '\_div\_symbol' | 'Cast' | 'Cast' | 
| '\_minus\_scalar' | 'Ceil' | 'Ceil' | 
| '\_minus\_scalar' | 'CheckNumerics' | 'Clip' | 
| '\_mul\_symbol' | 'Concat' | 'Concat' | 
| '\_Plus' | 'ConcatV2' | 'Constant' | 
| '\_plus\_scalar' | 'Conv2D' | 'ConstantFill' | 
| '\_pow\_scalar' | 'DecodeJpeg' | 'Conv' | 
| '\_rdiv\_scalar' | 'DepthwiseConv2dNative' | 'ConvTranspose' | 
| '\_rminus\_scalar' | 'Elu' | 'Div' | 
| '\_rpow\_scalar' | 'Equal' | 'Dropout' | 
| '\_rsub\_scalar' | 'ExpandDims' | 'Elu' | 
| '\_sub\_scalar' | 'Fill' | 'Exp' | 
| '\_sub\_symbol' | 'Floor' | 'FC' | 
| 'Activation' | 'FusedBatchNorm' | 'Flatten' | 
| 'add\_n' | 'FusedBatchNormV2' | 'Floor' | 
| 'argmax' | 'GatherV2' | 'Gather' | 
| 'BatchNorm' | 'Greater' | 'Gemm' | 
| 'BatchNorm\_v1' | 'GreaterEqual' | 'GlobalAveragePool' | 
| 'broadcast\_add' | 'Identity' | 'GlobalMaxPool' | 
| 'broadcast\_div' | 'LeakyRelu' | 'HardSigmoid' | 
| 'broadcast\_mul' | 'Less' | 'Identity' | 
| 'broadcast\_sub' | 'LessEqual' | 'ImageScaler' | 
| 'broadcast\_to' | 'LRN' | 'LeakyRelu' | 
| 'cast' | 'MatMul' | 'Log' | 
| 'Cast' | 'Maximum' | 'LogSoftmax' | 
| 'clip' | 'MaxPool' | 'LRN' | 
| 'Concat' | 'Mean' | 'MatMul' | 
| 'concat' | 'Minimum' | 'Max' | 
| 'Convolution' | 'Mul'  | 'MaxPool' | 
| 'Convolution\_v1' | 'NotEqual' | 'Mean' | 
| 'Crop' | 'Pack' | 'Min' | 
| 'Deconvolution' | 'Pad' | 'Mul' | 
| 'Dropout' | 'PadV2' | 'Neg' | 
| 'elemwise\_add' | 'Range' | 'Pad' | 
| 'elemwise\_div' | 'Rank' | 'ParametricSoftplus' | 
| 'elemwise\_mul' | 'Relu' | 'Pow' | 
| 'elemwise\_sub' | 'Relu6' | 'PRelu' | 
| 'exp' | 'Reshape' | 'Reciprocal' | 
| 'expand\_dims' | 'ResizeBilinear' | 'ReduceMax' | 
| 'flatten' | 'Rsqrt' | 'ReduceMean' | 
| 'Flatten' | 'Selu' | 'ReduceMin' | 
| 'FullyConnected' | 'Shape' | 'ReduceProd' | 
| 'LeakyReLU' | 'Sigmoid' | 'ReduceSum' | 
| 'LinearRegressionOutput' | 'Softmax' | 'Relu' | 
| 'log' | 'Square' | 'Reshape' | 
| 'log\_softmax' | 'Squeeze' | 'Scale' | 
| 'LRN' | 'StridedSlice' | 'ScaledTanh' | 
| 'max' | 'Sub' | 'Selu' | 
| 'max\_axis'  | 'Sum' | 'Shape' | 
| 'min' | 'Tanh' | 'Sigmoid' | 
| 'min\_axis' | 'Transpose' | 'Slice' | 
| 'negative' |  | 'Softmax' | 
| 'Pooling' |  | 'SoftPlus' | 
| 'Pooling\_v1' |  | 'Softsign' | 
| 'relu' |  | 'SpatialBN' | 
| 'Reshape' |  | 'Split' | 
| 'reshape' |  | 'Sqrt' | 
| 'reshape\_like' |  | 'Squeeze' | 
| 'sigmoid' |  | 'Sub' | 
| 'slice\_like' |  | 'Sum' | 
| 'SliceChannel' |  | 'Tanh' | 
| 'softmax' |  | 'ThresholdedRelu' | 
| 'Softmax' |  | 'Transpose' | 
| 'SoftmaxActivation' |  | 'Unsqueeze' | 
| 'SoftmaxOutput' |  | 'Upsample' | 
| 'split' |  |  | 
| 'sum' |  |  | 
| 'sum\_axis' |  |  | 
| 'tanh' |  |  | 
| 'transpose' |  |  | 
| 'UpSampling' |  |  | 

***Which model architectures does Neo support?***

Neo supports image classification models\.

***Which model format files does Neo read in?*** 

The file needs to be formatted as a tar\.gz file that includes additional files that depend on the type of framework\.
+ **TensorFlow**: Neo supports saved models and frozen models\.

  For *saved models*, Neo expects one \.pb or one \.pbtxt file and a variables directory that contains variables\. 

  For *frozen models*, Neo expect only one \.pb or \.pbtxt file\.
+ **PyTorch**: Neo expects one \.pth file containing the model definition\.
+ **MXNET**: Neo expects one symbol file \(\.json\) and one parameter file \(\.params\)\.
+ **XGBoost**: Neo expects one XGBoost model file \(\.model\) where the number of nodes in a tree can't exceed 2^31\.
+ **ONNX**: Neo expects one \.onnx file\.

***What input data shapes does Neo expect?***

Neo expects the name and shape of the expected data inputs for your trained model with a JSON dictionary form or list form\. The data inputs are framework specific\. 
+ `TensorFlow`: You must specify the name and shape \(NHWC format\) of the expected data inputs using a dictionary format for your trained model\. The dictionary formats required for the console and CLI are different\.
  + Examples for one input:
    + If using the console, `{"input":[1,1024,1024,3]}`
    + If using the CLI, `{\"input\":[1,1024,1024,3]}`
  + Examples for two inputs:
    + If using the console, `{"data1": [1,28,28,1], "data2":[1,28,28,1]}`
    + If using the CLI, `{\"data1\": [1,28,28,1], \"data2\":[1,28,28,1]}`
+ `MXNET/ONNX`: You must specify the name and shape \(NCHW format\) of the expected data inputs in order using a dictionary format for your trained model\. The dictionary formats required for the console and CLI are different\.
  + Examples for one input:
    + If using the console, `{"data":[1,3,1024,1024]}`
    + If using the CLI, `{\"data\":[1,3,1024,1024]}`
  + Examples for two inputs:
    + If using the console, `{"var1": [1,1,28,28], "var2":[1,1,28,28]} `
    + If using the CLI, `{\"var1\": [1,1,28,28], \"var2\":[1,1,28,28]}`
+ `PyTorch`: You can either specify the name and shape \(NCHW format\) of expected data inputs in order using a dictionary format for your trained model or you can specify the shape only using a list format\. The dictionary formats required for the console and CLI are different\. The list formats for the console and CLI are the same\.
  + Examples for one input in dictionary format:
    + If using the console, `{"input0":[1,3,224,224]}`
    + If using the CLI, `{\"input0\":[1,3,224,224]}`
  + Example for one input in list format: `[[1,3,224,224]]`
  + Examples for two inputs in dictionary format:
    + If using the console, `{"input0":[1,3,224,224], "input1":[1,3,224,224]}`
    + If using the CLI, `{\"input0\":[1,3,224,224], \"input1\":[1,3,224,224]} `
  + Example for two inputs in list format: `[[1,3,224,224], [1,3,224,224]]`
+ `XGBOOST`: input data name and shape are not needed\.

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