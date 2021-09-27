# Troubleshoot and Monitor Workloads in Amazon EMR<a name="studio-notebooks-emr-cluster-trouble-shoot"></a>

The following outlines instructions on how to set up SSH tunneling for Spark UI access\.

## Set up SSH Tunneling for Spark UI access<a name="studio-notebooks-emr-ssh-tunneling"></a>

If you would like to set up SSH tunneling to access the Spark UI then follow the steps below\. The link under Spark UI and Driver log shown in 6b would not be enabled unless the steps for SSH tunneling for Spark UI is followed\. 

1. [Option 1: Set up an SSH tunnel to the master node using local port forwarding](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel-local.html)

1. [Option 2, part 1: Set up an SSH tunnel to the master node using dynamic port forwarding](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel.html)

1. [Option 2, part 2: Configure proxy settings to view websites hosted on the master node](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-master-node-proxy.html)

For information on how to view web interfaces hosted on Amazon EMR clusters, see [View web interfaces on Amazon EMR Clusters](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-web-interfaces.html)\. 

## Processing Spark data<a name="studio-notebooks-emr-process-spark"></a>

For more information on how to process data using Spark from within a SageMaker Studio notebook, see this blog [Perform interactive data processing using Spark in SageMaker Studio Notebooks](https://aws.amazon.com/blogs/machine-learning/amazon-sagemaker-studio-notebooks-backed-by-spark-in-amazon-emr/)\. 

## Bring your own image<a name="studio-notebooks-emr-process-byoi"></a>

If you want to bring your own image, you need to install the following dependencies to your kernel\. Run the following `pip` commands along with the library names:

1. `pip install sparkmagic`\.

   `pip install sagemaker-studio-sparkmagic-lib`\.

   `pip install sagemaker-studio-analytics-extension`\.

If you want to connect to a Kerberos protected Amazon EMR then you will also need to install the kinit client\. Depending on your OS the command to install the kinit client will vary\. Here is the command for a Ubuntu/Debian based image: `apt-get install -y -qq krb5-user`\.