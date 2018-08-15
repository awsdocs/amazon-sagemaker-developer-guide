# Creating a Notebook Instance<a name="howitworks-create-ws"></a>

To create a notebook instance, use either the Amazon SageMaker console or the [CreateNotebookInstance](API_CreateNotebookInstance.md) API\. For an example of using the Amazon SageMaker console to create a notebook instance, see [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\.

After receiving the request, Amazon SageMaker does the following:
+ **Creates a network interface**—If you choose the optional VPC configuration, it creates the network interface in your VPC\. It uses the subnet ID that you provide in the request to determine which Availability Zone to create the subnet in\. Amazon SageMaker associates the security group that you provide in the request with the subnet\. For more information, see [Notebook Instance Security](appendix-additional-considerations.md)\. 
+ **Launches an ML compute instance**—Amazon SageMaker launches an ML compute instance in an Amazon SageMaker VPC\. It performs the configuration tasks that allow it to manage your notebook instance, and if you specified your VPC, it enables traffic between your VPC and the notebook instance\.
+ **Installs Anaconda packages and libraries for common deep learning platforms**—Amazon SageMaker installs all of the Anaconda packages that are included in the installer\. For more information, see [Anaconda package list](https://docs.anaconda.com/anaconda/packages/pkg-docs)\. In addition, Amazon SageMaker installs the TensorFlow and Apache MXNet deep learning libraries\. 
+ **Attaches an ML storage volume**—Amazon SageMaker attaches a 5\-GB ML storage volume to the ML compute instance\. You can use the volume to clean up the training dataset or to temporarily store other data to work with\. There is also 20 GB of instance storage available in the /tmp directory of each notebook instance\. This is not persistent storage\. When the instance is stopped or restarted, anything in the /tmp directory is deleted\.
+ **Copies example Jupyter notebooks**— These Python code examples illustrate model training and hosting exercises using various algorithms and training datasets\.

## Limit Access to a Notebook Instance by IP Address<a name="howitworks-create-filter-ip"></a>

You can limit access to a notebook instance to only IP addresses in a list that you specify\. For more information, see [Limit Access to a Notebook Instance by IP Address](howitworks-access-ws.md#nbi-ip-filter)\.