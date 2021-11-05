# Troubleshoot and Monitor Workloads in Amazon EMR<a name="studio-notebooks-emr-cluster-trouble-shoot"></a>

The following outlines instructions on how to set up SSH tunneling for Spark UI access\.

## Set up SSH tunneling for Spark UI access<a name="studio-notebooks-emr-ssh-tunneling"></a>

To set up SSH tunneling to access the Spark UI follow one of the two options in this section\. The link under Spark UI and Driver log shown in 6b can not be enabled until you follow the steps for SSH tunneling for Spark UI\. 

1. [Option 1: Set up an SSH tunnel to the master node using local port forwarding](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel-local.html)

1. [Option 2, part 1: Set up an SSH tunnel to the master node using dynamic port forwarding](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel.html)

1. [Option 2, part 2: Configure proxy settings to view websites hosted on the master node](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-master-node-proxy.html)

For information on how to view web interfaces hosted on Amazon EMR clusters, see [view web interfaces on Amazon EMR Clusters](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-web-interfaces.html)\. You can also visit your Amazon EMR console to get access to the Spark UI\.

## Processing Spark data<a name="studio-notebooks-emr-process-spark"></a>

For more information on how to process data using Spark from within a SageMaker Studio notebook, see this blog [Perform interactive data processing using Spark in SageMaker Studio Notebooks](https://aws.amazon.com/blogs/machine-learning/amazon-sagemaker-studio-notebooks-backed-by-spark-in-amazon-emr/)\. 