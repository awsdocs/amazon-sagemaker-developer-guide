# Constraints and considerations<a name="notebook-auto-run-constraints"></a>

Review the following constraints to ensure your notebook jobs complete successfully\. Studio uses Papermill to run notebooks\. You might need to update Jupyter notebooks to align to Papermill's requirements\. There are also restrictions on the content of LCC scripts and important details to understand regarding VPC configuration\.

**JupyterLab version support\.** JupyterLab versions 3\.0 and above are supported\.

**Using `pip install` with packages that require kernel restart\.** Papermill does not support calling `pip install` to install packages that require a kernel restart\. In this situation, use `pip install` in an initialization script\. For a package installation that does not require kernel restart, you can still include `pip install` in the notebook\. 

**Papermill assumes specific kernel and language names registered with Jupyter\.** Papermill registers a translator for specific kernels and languages\. If you bring your own instance \(BYOI\), use a standard kernel name as shown in the following snippet:

```
papermill_translators.register("python", PythonTranslator)
papermill_translators.register("R", RTranslator)
papermill_translators.register("scala", ScalaTranslator)
papermill_translators.register("julia", JuliaTranslator)
papermill_translators.register("matlab", MatlabTranslator)
papermill_translators.register(".net-csharp", CSharpTranslator)
papermill_translators.register(".net-fsharp", FSharpTranslator)
papermill_translators.register(".net-powershell", PowershellTranslator)
papermill_translators.register("pysparkkernel", PythonTranslator)
papermill_translators.register("sparkkernel", ScalaTranslator)
papermill_translators.register("sparkrkernel", RTranslator)
papermill_translators.register("bash", BashTranslator)
```

**Parameters and environment variable limits\.** When you create your notebook job, it receives the parameters and environment variables you specify\. You can pass up to 100 parameters\. Each parameter name can be up to 256 characters long, and the associated value can be up to 2500 characters long\. If you pass environment variables, you can pass up to 28 variables\. The variable name and associated value can be up to 512 characters long\. If you need more than 28 environment variables, use additional environment variables in an initialization script which has no limit on the number of environment variables you can use\.

**Image and kernel support\.** The driver that launches your notebook job assumes the following:
+ A base Python runtime environment is installed in the Studio or bring\-your\-own \(BYO\) images and is the default in the shell\.
+ The base Python runtime environment includes the Jupyter client with kernelspecs properly configured\.
+ The base Python runtime environment includes the `pip` function so the notebook job can install system dependencies\.
+ For images with multiple environments, your initialization script should switch to the proper kernel\-specific environment before installing notebook\-specific packages\. You should switch back to the default Python runtime environment, if different from the kernel runtime environment, after configuring the kernel Python runtime environment\.

The driver that launches your notebook job is a bash script, and Bash v4 must be available at /bin/bash\. 

**Root privileges on bring\-your\-own\-images \(BYOI\)\.** You must have root privileges on your own Studio images, either as the root user or through `sudo` access\. If you are not a root user but accessing root privileges through `sudo`, use **1000/100** as the `UID/GID`\.

**Only private VPC subnets used during job creation\.** If you use a VPC, Studio uses your private subnets to create your job\. Specify one to five private subnets \(and 1–15 security groups\)\.

If you use a VPC with private subnets, you must choose one of the following options to ensure the notebook job can connect to dependent services or resources:
+ If the job needs access to an AWS service that supports interface VPC endpoints, create an endpoint to connect to the service\. For a list of services that support interface endpoints, see [AWS services that integrate with AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/aws-services-privatelink-support.html)\. For information about creating an interface VPC endpoint, see [Access an AWS service using an interface VPC endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html)\. At minimum, an Amazon S3 VPC endpoint gateway must be provided\.
+ If a notebook job needs access to an AWS service that doesn't support interface VPC endpoints or to a resource outside of AWS, create a NAT gateway and configure your security groups to allow outbound connections\. For information about setting up a NAT gateway for your VPC, see *VPC with public and private Subnets \(NAT\)* in the [Amazon Virtual Private Cloud User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)\.

**Service limits\.** Since the notebook job scheduler is built from SageMaker Pipelines, SageMaker Training, and Amazon EventBridge services, your notebook jobs are subject to their service\-specific quotas\. If you exceed these quotas, you may see error messages related to these services\. For example, there are limits for how many pipelines you can run at one time, and how many rules you can set up for a single event bus\. For more information about SageMaker quotas, see [Amazon SageMaker Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html)\. For more information about EventBridge quotas, see [Amazon EventBridge Quotas](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-quota.html)\.