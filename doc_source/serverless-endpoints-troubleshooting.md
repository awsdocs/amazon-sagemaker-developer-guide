# Troubleshooting<a name="serverless-endpoints-troubleshooting"></a>

If you are having trouble with Serverless Inference, refer to the following troubleshooting tips\.

## Container issues<a name="serverless-endpoints-troubleshooting-containers"></a>

If the container you use for a serverless endpoint is the same one you used on an instance\-based endpoint, your container may not have permissions to write files\. This can happen for the following reasons:
+ Your serverless endpoint fails to create or update due to a ping health check failure\.
+ The Amazon CloudWatch logs for the endpoint show that the container is failing to write to some file or directory due to a permissions error\.

To fix this issue, you can try to add read, write, and execute permissions for `other` on the file or directory and then rebuild the container\. You can perform the following steps to complete this process:

1. In the Dockerfile you used to build your container, add the following command: `RUN chmod o+rwX <file or directory name>`

1. Rebuild the container\.

1. Upload the new container image to Amazon ECR\.

1. Try to create or update the serverless endpoint again\.