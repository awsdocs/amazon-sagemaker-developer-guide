# Additional options<a name="create-notebook-auto-execution-advanced"></a>

Studio tries to infer details about your job's infrastructure settings so you can launch your jobs quickly without worrying about the backend details\. However, if your job requires further customization, you can override the Studio provided defaults in the **Create Job** form\.

If you choose the **Additional options** dropdown list in the **Create Job** form, you can customize the following options:


| Custom option | Description | 
| --- | --- | 
| Image | The kernel image used to run the notebook job\. Studio attempts to infer this field from your notebook’s current image\. If Studio cannot infer this value, the form displays a validation error requiring you to specify it\. For a list of images supported by the notebook scheduler, see [Available Amazon SageMaker Images](notebooks-available-images.md)\. | 
| Kernel | The Jupyter kernel used to run the notebook job\. Studio infers this field from your notebook’s current kernel\. If Studio cannot infer this value, the form displays a validation error requiring you to specify it\. | 
| Role ARN | The role’s Amazon Resource Name \(ARN\)\. This role is used with the notebook job and defaults to the Studio execution role\. If Studio cannot infer this value, the **Role ARN** field will be blank\. In this case, insert the ARN you want to use\.  | 
| Input folder | The folder containing your inputs\. If you don’t specify a folder, Studio creates a default Amazon S3 bucket for your inputs\. Studio puts the job inputs, including the input notebook and any optional start\-up or initialization scripts, in this folder\. | 
| Output folder | The folder containing your outputs\. If you don’t specify a folder, Studio creates a default Amazon S3 bucket for your outputs\. | 
| Environment variables | Any existing environment variables that you want to override, or new environment variables that you want to introduce and use in your notebook\. | 
| Start\-up script | A script preloaded in the notebook startup menu that you can choose to run before you run the notebook\. Your admin must set this up with a Lifecycle Configuration \(LCC\) script\.  A start\-up script runs in a shell outside of the Studio environment\. Therefore, this script cannot depend on the Studio local storage, environment variables, or app metadata \(in `/opt/ml/metadata`\)\. Also, if you use a start\-up script and an initialization script, the start\-up script runs first\.   | 
| Initialization script | An alternative to using an LCC script\. This is a path to a local script you can run when your notebook starts up\. If you don't provide an absolute path, Studio assumes your path is relative to /home/sagemaker\-user\. An initialization script is sourced from the same shell as the notebook job so the script can access the Studio local storage, environment variables, and app metadata \(in `/opt/ml/metadata`\)\. This is not the case for a start\-up script described previously\. Also, if you use a start\-up script and an initialization script, the start\-up script runs first\.    | 
| Configure job encryption | Check this box if you want to encrypt your notebook job outputs, job instance volume, or both\. This option is not selected by default\. If left unchecked, Studio encrypts the job outputs with the account's default KMS key and does not encrypt the job instance volume\. | 
| Output encryption KMS key | If you checked Configure job encryption, insert a KMS key if you want to customize the encryption key used for your notebook job outputs\. If you do not specify this field, Studio encrypts the notebook job outputs with SSE\-KMS using the default Amazon S3 KMS key\. Also, if you create the S3 bucket yourself and use encryption, Studio respects whatever encryption type you use\. | 
| Job instance volume encryption KMS key | If you checked Configure job encryption, insert a KMS key if you want to encrypt your job instance volume\. | 

If Studio detects that you configured a Virtual Private Cloud \(VPC\), you can also customize the following options:


| Custom option | Description | 
| --- | --- | 
| Use a Virtual Private Cloud to run this job | Check this box if you want to use a VPC\. At the minimum, create the following VPC endpoints to enable your notebook job to privately connect to those AWS resources: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/create-notebook-auto-execution-advanced.html)If you choose to use a VPC, you need to specify at least one private subnet and at least one security group in the following options\. If you don’t use any private subnets, you need to consider other configuration options\. For details, see Public VPC subnets not supported in [Constraints and considerations](notebook-auto-run-constraints.md)\. | 
| Subnet\(s\) | Your subnets\. This defaults to the subnets associated with the Studio domain\. This field must contain at least one and at most five, and all the subnets you provide should be private\. For details, see Public VPC subnets not supported in [Constraints and considerations](notebook-auto-run-constraints.md)\. | 
| Security group\(s\) | Your security groups\. This field must contain at least one and at most 15\. For details, see Public VPC subnets not supported in [Constraints and considerations](notebook-auto-run-constraints.md)\. | 