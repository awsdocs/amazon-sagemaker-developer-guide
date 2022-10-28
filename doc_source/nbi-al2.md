# Amazon Linux 2 vs Amazon Linux notebook instances<a name="nbi-al2"></a>

Amazon SageMaker notebook instances currently support Amazon Linux 2 \(AL2\) and Amazon Linux \(AL1\) operating systems\. You can select the operating system that your notebook instances is based on when you create the notebook instance\. Notebook instances created before 08/18/2021 automatically run on AL1\.

Notebook instances based on AL1 will enter a maintenance phase as of 12/01/2022\. To replace AL1, you now have the option to create Amazon SageMaker notebook instances with AL2\. The AL1 maintenance phase also coincides with the deprecation of Python 2 and Chainer\. Notebooks based on AL2 do not have managed Python 2 and Chainer kernels\.

## AL1 Maintenance Phase Plan<a name="nbi-al2-deprecation"></a>

The following table is a timeline for when AL1 enters its extended maintenance phase\.


|  Date  |  Description  | 
| --- | --- | 
|  08/18/2021  |  Notebook instances based on AL2 are launched\. Newly launched notebook instances still default to AL1\. AL1 is supported with security patches and updates, but no new features\. You can choose between the two operating systems when launching a new notebook instance\.  | 
|  10/31/2022  |  The default platform identifier for SageMaker notebook instances changes from Amazon Linux \(al1\-v1\) to Amazon Linux 2 \(al2\-v2\)\. You can choose between the two operating systems when launching a new notebook instance\.  | 
|  12/01/2022  |  AL1 is no longer supported with non\-critical security patches and updates\. AL1 still receives fixes for [critical](https://nvd.nist.gov/vuln-metrics/cvss#) security\-related issues\. You can still launch instances on AL1, but assume the risks associated with using an unsupported operating system\.  | 
|  02/01/2023  |  AL1 is no longer an available option for new notebook instance creation\. After this date, customers can create notebook instances with the AL2 platform identifiers\. Existing al1\-v1 notebook instances are not be affected\.   | 

## Available Kernels<a name="nbi-al2-env"></a>

`notebook-al1-v1`: The following kernels are available in notebook instances based on the Amazon Linux platform\. These notebook instances support JupyterLab version 1\. For information about JupyterLab versions, see [JupyterLab versioning](nbi-jl.md)\.


|  Kernel Name  | 
| --- | 
|  R  | 
|  Sparkmagic \(PySpark\)  | 
|  Sparkmagic \(Spark\)  | 
|  Sparkmagic \(SparkR\)  | 
|  conda\_amazonei\_mxnet\_p27  | 
|  conda\_amazonei\_mxnet\_p36  | 
|  conda\_amazonei\_pytorch\_latest\_p36  | 
|  conda\_amazonei\_tensorflow2\_p27  | 
|  conda\_amazonei\_tensorflow2\_p36  | 
|  conda\_amazonei\_tensorflow\_p27  | 
|  conda\_amazonei\_tensorflow\_p36  | 
|  conda\_chainer\_p27  | 
|  conda\_chainer\_p36  | 
|  conda\_mxnet\_latest\_p37  | 
|  conda\_mxnet\_p27  | 
|  conda\_mxnet\_p36  | 
|  conda\_python2  | 
|  conda\_python3  | 
|  conda\_pytorch\_latest\_p36  | 
|  conda\_pytorch\_p27  | 
|  conda\_pytorch\_p36  | 
|  conda\_tensorflow2\_p36  | 
|  conda\_tensorflow\_p27  | 
|  conda\_tensorflow\_p36  | 

`notebook-al2-v1`: The following kernels are available in notebook instances based on the Amazon Linux 2 platform\. These notebook instances support JupyterLab version 1\. For information about JupyterLab versions, see [JupyterLab versioning](nbi-jl.md)\.


|  Kernel Name  | 
| --- | 
|  R  | 
|  Sparkmagic \(PySpark\)  | 
|  Sparkmagic \(Spark\)  | 
|  Sparkmagic \(SparkR\)  | 
|  conda\_amazonei\_mxnet\_p36  | 
|  conda\_amazonei\_pytorch\_latest\_p37  | 
|  conda\_amazonei\_tensorflow2\_p36  | 
|  conda\_mxnet\_p37  | 
|  conda\_python3  | 
|  conda\_pytorch\_p38  | 
|  conda\_tensorflow2\_p38  | 

`notebook-al2-v2`: The following kernels are available in notebook instances based on the Amazon Linux 2 platform\. These notebook instances support JupyterLab version 3\. For information about JupyterLab versions, see [JupyterLab versioning](nbi-jl.md)\.


|  Kernel Name  | 
| --- | 
|  R  | 
|  Sparkmagic \(PySpark\)  | 
|  Sparkmagic \(Spark\)  | 
|  Sparkmagic \(SparkR\)  | 
|  conda\_amazonei\_pytorch\_latest\_p37  | 
|  conda\_mxnet\_p37  | 
|  conda\_python3  | 
|  conda\_pytorch\_p38  | 
|  conda\_tensorflow2\_p38  | 

## Migrating to Amazon Linux 2<a name="nbi-al2-upgrade"></a>

Your existing notebook instance is not automatically migrated to Amazon Linux 2\. To upgrade your notebook instance to Amazon Linux 2, you must create a new notebook instance, replicate your code and environment, and delete your old notebook instance\. For more information, see the [Amazon Linux 2 migration blog](https://aws.amazon.com/blogs/machine-learning/migrate-your-work-to-amazon-sagemaker-notebook-instance-with-amazon-linux-2/ )\.