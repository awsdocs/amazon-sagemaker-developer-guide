# Creating and Associating a Lifecycle Configuration<a name="studio-lcc-create"></a>

Amazon SageMaker Studio provides interactive applications that enable Studio's visual interface, code authoring, and run experience\. This series shows how to create a lifecycle configuration and associate it with Amazon SageMaker Studio\.

Application types can be either `JupyterServer` or `KernelGateway`\. 
+ **`JupyterServer`applications:** This application type enables access to the visual interface for Studio\. Every user in Studio gets their own Jupyter Server application\.
+ **`KernelGateway` applications:** This application type enables access to the code run environment and kernels for your Studio notebooks and terminals\. For more information, see [Jupyter Kernel Gateway](https://jupyter-kernel-gateway.readthedocs.io/en/latest/)\.

For more information about Studio's architecture and Studio applications, see [Use Amazon SageMaker Studio Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks.html)\.

**Topics**
+ [Create a Lifecycle Configuration from the AWS CLI](studio-lcc-create-cli.md)
+ [Create a Lifecycle Configuration from the SageMaker Console](studio-lcc-create-console.md)