# Save Updated Sample Notebooks in a New Location<a name="save-notebook-updates"></a>

Amazon SageMaker provides several sample notebooks in your notebook instance\. Each of these samples provides step\-by\-step instructions for training a model, and deploying and validating it\. As you explore these samples, you might modify their code\. We recommend that you move the modified files and folders out of the `/sample-notebooks` folder\. 

When you stop a notebook instance, Amazon SageMaker frees up the resource \(a machine learning instance\)\. When you restart your notebook instance, Amazon SageMaker provisions a new machine learning instance with the latest sample notebooks and an updated version of the Amazon Machine Image \(AMI\)\. 

As a result, when you stop an instance, you lose changes that you made in any files in the `/sample-notebooks` folder\. To save your changes, move the relevant files and folders out of the `/sample-notebook` folder, to any other folder within the `SageMaker` folder\. The `SageMaker` folder is the default location when you open a notebook instance\. Amazon SageMaker saves any files within the `SageMaker` folder, except for anything in the `/sample-notebooks` subfolder\. 

To move files and folders, use the **Move** command in the Jupyter dashboard\.