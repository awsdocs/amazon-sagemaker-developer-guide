# Troubleshoot Errors<a name="neo-troubleshooting"></a>

This section contains information about how to understand and prevent common errors, the error messages they generate, and guidance on how to resolve these errors\. Before moving on, ask yourself the following questions:

 **Did you encounter an error before you deployed your model?** If yes, see [Troubleshoot Neo Compilation Errors](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting-compilation.html)\. 

 **Did you encounter an error after you compiled your model?** If yes, see [Troubleshoot Neo Inference Errors](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting-inference.html)\. 

**Did you encounter an error trying to compile your model for Ambarella devices?** If yes, see [Troubleshoot Ambarella Errors](neo-troubleshooting-target-devices-ambarella.md)\.

## Error Classification Types<a name="neo-error-messages"></a>

This list classifies the *user errors* you can receive from Neo\. These include access and permission errors and load errors for each of the supported frameworks\. All other errors are *system errors*\.

### Client permission error<a name="collapsible-section-1"></a>

 Neo passes the errors for these straight through from the dependent service\. 
+ *Access Denied* when calling sts:AssumeRole
+ *Any 400* error when calling Amazon S3 to download or upload a client model
+ *PassRole* error

### Load error<a name="collapsible-section-2"></a>

Assuming that the Neo compiler successfully loaded \.tar\.gz from Amazon S3, check whether the tarball contains the necessary files for compilation\. The checking criteria is framework\-specific: 
+ **TensorFlow**: Expects only protobuf file \(\*\.pb or \*\.pbtxt\)\. For saved models, expects one variables folder\. 
+ **Pytorch**: Expect only one pytorch file \(\*\.pth\)\.
+ **MXNET**: Expect only one symbol file \(\*\.json\) and one parameter file \(\*\.params\)\.
+ **XGBoost**: Expect only one XGBoost model file \(\*\.model\)\. The input model has size limitation\.

### Compilation error<a name="collapsible-section-3"></a>

Assuming that the Neo compiler successfully loaded \.tar\.gz from Amazon S3, and that the tarball contains necessary files for compilation\. The checking criteria is: 
+ **OperatorNotImplemented**: An operator has not been implemented\.
+ **OperatorAttributeNotImplemented**: The attribute in the specified operator has not been implemented\. 
+ **OperatorAttributeRequired**: An attribute is required for an internal symbol graph, but it is not listed in the user input model graph\. 
+ **OperatorAttributeValueNotValid**: The value of the attribute in the specific operator is not valid\. 

**Topics**
+ [Error Classification Types](#neo-error-messages)
+ [Troubleshoot Neo Compilation Errors](neo-troubleshooting-compilation.md)
+ [Troubleshoot Neo Inference Errors](neo-troubleshooting-inference.md)
+ [Troubleshoot Ambarella Errors](neo-troubleshooting-target-devices-ambarella.md)