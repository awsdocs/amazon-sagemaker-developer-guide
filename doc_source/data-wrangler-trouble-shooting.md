# Troubleshoot<a name="data-wrangler-trouble-shooting"></a>

If an issue arises when using Amazon SageMaker Data Wrangler, we recommend you do the following:
+ If an error message is provided, read the message and resolve the issue it reports if possible\.
+ Make sure the IAM role of your Studio user has the required permissions to perform the action\. For more information, see [Security and Permissions](data-wrangler-security.md)\.
+ If the issue occurs when you are trying to import from another AWS service, such as Amazon Redshift or Athena, make sure that you have configured the necessary permissions and resources to perform the data import\. For more information, see [Import](data-wrangler-import.md)\.
+ If you're still having issues, choose **Get help** at the top right of your screen to reach out to the Data Wrangler team\. For more information, see the following images\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/get-help/get-help.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/get-help/get-help-forms.png)

As a last resort, you can try restarting the kernel on which Data Wrangler is running\. 

1. Save and exit the \.flow file for which you want to restart the kernel\. 

1. Select the ****Running Terminals and Kernels**** icon, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/stop-kernel-option.png)

1. Select the **Stop** icon to the right of the \.flow file for which you want to terminate the kernel, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/stop-kernel.png)

1. Refresh the browser\. 

1. Reopen the \.flow file on which you were working\. 

## Troubleshooting issues with Amazon EMR<a name="data-wrangler-trouble-shooting-emr"></a>

Use the following information to help you troubleshoot errors that might come up when you're using Amazon EMR\.
+ Connection failure – If the connection fails with the following message `The IP address of the EMR cluster isn't private error message`, your Amazon EMR cluster might not have been launched in a private subnet\. As a security best practice, Data Wrangler only supports connecting to private Amazon EMR clusters\. Choose a private EC2 subnet you launch an EMR cluster\.
+ Connection hanging and timing out – The issue is most likely due to a network connectivity issue\. After you start connecting to the cluster, the screen doesn't refresh\. After about 2 minutes, you might see the following error `JdbcAddConnectionError: An error occurred when trying to connect to presto: xxx: Connect to xxx failed: Connection timed out (Connection timed out) will display on top of the screen.`\.

  The errors might have two root causes:
  + The Amazon EMR and Amazon SageMaker Studio are in different VPCs\. We recommend launching both Amazon EMR and Studio in the same VPC\. You can also use VPC peering\. For more information, see [What is VPC peering?](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)\.
  + The Amazon EMR master security group lacks the inbound traffic rule for the security group of Amazon SageMaker Studio on the port used for Presto\. To resolve the issue, allow inbound traffic on port 8889\.
+ Connection fails due to the connection type being misconfigured – You might see the following error message: ` Data Wrangler couldn't create a connection to {connection_source} successfully. Try connecting to {connection_source} again. For more information, see Troubleshoot. If you’re still experiencing issues, contact support. `

  Check the authentication method\. The authentication method that you've specified in Data Wrangler should match the authentication method that you're using on the cluster\.
+ You don't have HDFS permissions for LDAP authentication – Use the following guidance to resolve the issue [Set up HDFS Permissions using Linux Credentials](https://docs.aws.amazon.com/whitepapers/latest/teaching-big-data-skills-with-amazon-emr/set-up-hdfs-permissions-using-linux-credentials.html)\. You can log into the cluster using the following commands:

  ```
  hdfs dfs -mkdir /user/USERNAME
  hdfs dfs -chown USERNAME:USERNAME /user/USERNAME
  ```
+ LDAP authentication missing connection key error – You might see the following error message: `Data Wrangler couldn't connect to EMR hive successfully. JDBC connection is missing required connection key(s): PWD`\.

  For LDAP authentication, you must specify both a username and a password\. The JDBC URL stored in Secrets Manager is missing property `PWD`\.
+ When you're troubleshooting the LDAP configuration: We recommend making sure that the LDAP authenticator \(LDAP server\) is correctly configured to connect to the Amazon EMR cluster\. Use the `ldapwhoami` command to help you resolve the configuration issue\. The following are example commands that you can run:
  + For LDAPS – `ldapwhoami -x -H ldaps://ldap-server`
  + For LDAP – `ldapwhoami -x -H ldap://ldap-server`

  Either command should return `Anonymous` if you've configured the authenticator successfully\.