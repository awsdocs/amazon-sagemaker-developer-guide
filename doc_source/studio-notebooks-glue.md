# Prepare Data using AWS Glue Interactive Sessions<a name="studio-notebooks-glue"></a>

AWS Glue Interactive Sessions is a serverless service that equips you with the tools to collect, transform, cleanse, and prepare the data that will populate your data lakes and pipelines\. Glue Interactive Sessions provides an on\-demand, serverless Apache Spark runtime environment that data scientists and engineers can use to rapidly build, test, and run data preparation and analytics applications\.

Starting a Glue interactive session from a SageMaker Studio notebook is simple\. When you create your Studio notebook, choose the built\-in `Glue PySpark` or `Glue Spark` kernel and start coding in your interactive, serverless Spark session in just seconds\. You don’t have worry about provisioning or managing complex compute cluster infrastructure\. After initialization, you can quickly browse the Glue data catalog, run large queries, and interactively analyze and prepare data using Spark, all within your Studio notebook\. You can then use the prepared data to build, train, tune, and deploy models using the purpose\-built ML tools within SageMaker Studio\.

Before you start your AWS Glue interactive session in SageMaker Studio, you need to set the appropriate roles and policies\. You may also need access to additional resources, such as Amazon S3, which may require additional policies\. For more information about required and additional IAM policies, see [Permissions for Glue Interactive Sessions in SageMaker Studio](getting-started-glue-sm.md#glue-sm-iam)\.

SageMaker Studio provides a default configuration for your AWS Glue interactive session, but you can use Glue’s full catalog of Jupyter magic commands to further customize your environment\. For information about the default and additional Jupyter magics that you can use in your Glue interactive session, see [Configure your Glue interactive session in SageMaker Studio](getting-started-glue-sm.md#glue-sm-magics)\.

The supported images and kernels for connecting to a Glue interactive session are as follows:
+ Images: SparkAnalytics 1\.0
+ Kernel: PySpark and Spark

**Prerequisites:**

The SparkAnalytics image that you select to launch your Glue session in Studio is a combination of two frameworks \- the SparkMagic framework \(used with Amazon EMR\), and AWS Glue\. For this reason, the prerequisites for both frameworks apply\. However, you do not have to set up the EMR cluster if you only plan to use Glue Interactive Sessions\. Before you start your first Glue interactive session in Studio, complete the following:
+ Complete the prerequisites required to use the SparkMagic image\. For a list of the prerequisites, see the Prerequisites section in [Prepare Data at Scale with Studio Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-notebooks-emr-cluster.html)\.
+ Create an execution role with permissions for both AWS Glue and SageMaker Studio\. Add the managed policies `SageMakerFullAccess` and `AwsGlueSessionUserRestrictedServiceRole`, and create a custom policy that includes permissions `sts:GetCallerIdentity`, `iam:GetRole`, and `IAM:Passrole`\. For instructions about how to create the necessary permissions, see [Permissions for Glue Interactive Sessions in SageMaker Studio](getting-started-glue-sm.md#glue-sm-iam)\.
+ Create a SageMaker domain with the execution role you created\. For instructions about how to create a domain, see [Onboard to Amazon SageMaker Domain Using IAM](onboard-iam.md)\.