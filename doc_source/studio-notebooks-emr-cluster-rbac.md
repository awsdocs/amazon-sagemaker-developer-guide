# Connect to an Amazon EMR Cluster from Studio Using Runtime IAM Roles<a name="studio-notebooks-emr-cluster-rbac"></a>

When you connect to an Amazon EMR cluster from your Amazon SageMaker Studio notebook, you can visually browse a list of IAM roles, known as runtime roles, and select one on the fly\. Subsequently, all your Apache Spark, Apache Hive, or Presto jobs created from your Studio notebook access only the data and resources permitted by policies attached to the runtime role\. Also, when data is accessed from data lakes managed with AWS Lake Formation, you can enforce table\-level and column\-level access using policies attached to the runtime role\.

With this capability, you and your teammates can connect to the same cluster, each using a runtime role scoped with permissions matching your individual level of access to data\. Your sessions are also isolated from one another on the shared cluster\. With this ability to control fine\-grained access to data on the same shared cluster, you can simplify provisioning of Amazon EMR clusters, reducing operational overhead and saving costs\.

To try out this new feature, see [ Apply fine\-grained data access controls with AWS Lake Formation and Amazon EMR from Amazon SageMaker Studio ](http://aws.amazon.com/blogs/machine-learning/apply-fine-grained-data-access-controls-with-aws-lake-formation-and-amazon-emr-from-amazon-sagemaker-studio/)\. This blog post helps you set up a demo environment where you can try using preconfigured runtime roles to connect to Amazon EMR clusters\.

## Cross\-account connection scenarios<a name="studio-notebooks-emr-cluster-rbac-scen"></a>

Runtime role authentication supports a variety of cross\-account connection scenarios when your data resides outside of your Studio account\. The following image shows three different ways you can assign your Amazon EMR cluster, data, and even Amazon EMR execution role between your Studio and data accounts: 

![\[Cross-account scenarios supported by runtime IAM role authentication.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio-emr-rbac-scenarios.png)

In option 1, your Amazon EMR cluster and Amazon EMR execution role are in a separate data account from your Studio account\. You define a separate Amazon EMR access role permission policy which grants permission to your Studio execution role to assume the Amazon EMR access role\. The Amazon EMR access role then calls the Amazon EMR API `GetClusterSessionCredentials` on behalf of your Studio execution role, giving you access to the cluster\.

In option 2, your Amazon EMR cluster and Amazon EMR execution role are in your Studio account\. Your Studio execution role has permission to use the Amazon EMR API `GetClusterSessionCredentials` to gain access to your cluster\. To access the Amazon S3 bucket, give the Amazon EMR execution role cross\-account Amazon S3 bucket access permissions — you grant these permissions within your Amazon S3 bucket policy\.

In option 3, your Amazon EMR clusters are in your Studio account, and the Amazon EMR execution role is in the data account\. Your Studio execution role has permission to use the Amazon EMR API `GetClusterSessionCredentials` to gain access to your cluster\. Add the Amazon EMR execution role into the execution role configuration JSON\. Then you can select the role in the UI when you choose your cluster\. For details about how to set up your execution role configuration JSON file, see [Preload your execution roles into Studio](studio-notebooks-emr-cluster-iam.md#studio-notebooks-emr-cluster-iam-preload)\.

## Prerequisites<a name="studio-notebooks-emr-cluster-rbac-prereq"></a>

Before you get started, make sure you meet the following prerequisites:
+ Use Amazon EMR version 6\.9 or above\.
+ Use JupyterLab version 3 in the Studio Jupyter server application configuration\. This version supports Studio connection to Amazon EMR clusters using runtime roles\.
+ Allow the use of runtime roles in your cluster’s security configuration\. For more information, see [ Runtime roles for Amazon EMR steps](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-steps-runtime-roles.html)\.
+ Create a notebook with any of the kernels listed in [Connect to an Amazon EMR Cluster from Studio](studio-notebooks-emr-cluster-connect.md)\.
+ Make sure you review the instructions in [Set Up Studio to Use Runtime IAM Roles](studio-notebooks-emr-cluster-iam.md) to configure runtime roles with Studio\.