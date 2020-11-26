# Control an Amazon EMR Spark Instance Using a Notebook<a name="nbi-lifecycle-config-emr"></a>

You can use a notebook instance created with a custom lifecycle configuration script to access AWS services from your notebook\. For example, you can create a script that lets you use your notebook with Sparkmagic to control other AWS resources, such as an Amazon EMR instance\. You can then use the Amazon EMR instance to process your data instead of running the data analysis on your notebook\. This allows you to create a smaller notebook instance because you won't use the instance to process data\. This is helpful when you have large datasets that would require a large notebook instance to process the data\.

The process requires three procedures using the Amazon SageMaker console:
+ Create the Amazon EMR Spark instance
+ Create the Jupyter Notebook
+ Test the notebook\-to\-Amazon EMR connection

**To create an Amazon EMR Spark instance that can be controlled from a notebook using Sparkmagic**

1. Open the Amazon EMR console at [https://console\.aws\.amazon\.com/elasticmapreduce/](https://console.aws.amazon.com/elasticmapreduce/)\.

1. In the navigation pane, choose **Create cluster**\.

1. On the **Create Cluster \- Quick Options** page, under **Software configuration**, choose **Spark: Spark 2\.4\.4 on Hadoop 2\.8\.5 YARN with Ganglia 3\.7\.2 and Zeppelin 0\.8\.2**\.

1. Set additional parameters on the page and then choose **Create cluster**\.

1. On the **Cluster** page, choose the cluster name that you created\. Note the **Master Public DNS**, the **EMR master's security group**, and the VPC name and subnet ID where the EMR cluster was created\. You will use these values when you create a notebook\.

**To create a notebook that uses Sparkmagic to control an Amazon EMR Spark instance**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, under **Notebook instances**, choose **Create notebook**\.

1. Enter the notebook instance name and choose the instance type\.

1. Choose **Additional configuration**, then, under **Lifecycle configuration**, choose **Create a new lifecycle configuration**\.

1. Add the following code to the lifecycle configuration script:

   ```
   # OVERVIEW
   # This script connects an Amazon EMR cluster to an Amazon SageMaker notebook instance that uses Sparkmagic.
   #
   # Note that this script will fail if the Amazon EMR cluster's master node IP address is not reachable.
   #   1. Ensure that the EMR master node IP is resolvable from the notebook instance.
   #      One way to accomplish this is to have the notebook instance and the Amazon EMR cluster in the same subnet.
   #   2. Ensure the EMR master node security group provides inbound access from the notebook instance security group.
   #       Type        - Protocol - Port - Source
   #       Custom TCP  - TCP      - 8998 - $NOTEBOOK_SECURITY_GROUP
   #   3. Ensure the notebook instance has internet connectivity to fetch the SparkMagic example config.
   #
   # https://aws.amazon.com/blogs/machine-learning/build-amazon-sagemaker-notebooks-backed-by-spark-in-amazon-emr/
   
   # PARAMETERS
   EMR_MASTER_IP=your.emr.master.ip
   
   
   cd /home/ec2-user/.sparkmagic
   
   echo "Fetching Sparkmagic example config from GitHub..."
   wget https://raw.githubusercontent.com/jupyter-incubator/sparkmagic/master/sparkmagic/example_config.json
   
   echo "Replacing EMR master node IP in Sparkmagic config..."
   sed -i -- "s/localhost/$EMR_MASTER_IP/g" example_config.json
   mv example_config.json config.json
   
   echo "Sending a sample request to Livy.."
   curl "$EMR_MASTER_IP:8998/sessions"
   ```

1. In the `PARAMETERS` section of the script, replace `your.emr.master.ip` with the Master Public DNS name for the Amazon EMR instance\.

1. Choose **Create configuration**\.

1. On the **Create notebook** page, choose **Network \- optional**\.

1. Choose the VPC and subnet where the Amazon EMR instance is located\.

1. Choose the security group used by the Amazon EMR master node\.

1. Choose **Create notebook instance**\.

While the notebook instance is being created, the status is **Pending**\. After the instance has been created and the lifecycle configuration script has successfully run, the status is **InService**\.

**Note**  
If the notebook instance can't connect to the Amazon EMR instance, SageMaker can't create the notebook instance\. The connection can fail if the Amazon EMR instance and notebook are not in the same VPC and subnet, if the Amazon EMR master security group is not used by the notebook, or if the Master Public DNS name in the script is incorrect\. 

**To test the connection between the Amazon EMR instance and the notebook**

1.  When the status of the notebook is **InService**, choose **Open Jupyter** to open the notebook\.

1. Choose **New**, then choose **Sparkmagic \(PySpark\)**\.

1. In the code cell, enter **%%info** and then run the cell\.

   The output should be similar to the following

   ```
   Current session configs: {'driverMemory': '1000M', 'executorCores': 2, 'kind': 'pyspark'}
                       No active sessions.
   ```