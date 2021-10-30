# Troubleshooting your Docker containers<a name="docker-containers-troubleshooting"></a>

The following are common errors that you might run into when using Docker containers with SageMaker\. Each error is followed by a solution to the error\. 
+ ** Error: SageMaker has lost the Docker daemon\.**

   To fix this error, restart Docker using the following command\.

  ```
  sudo service docker restart
  ```
+ ** Error: The `/tmp` directory of your Docker container has run out of space\.**

  Docker containers use the `/` and `/tmp` partitions to store code\. These partitions can fill up easily when using large code modules in local mode\. The SageMaker Python SDK supports specifying a custom temp directory for your local mode root directory to avoid this issue\.

  To specify the custom temp directory in the EBS volume storage, create a file at the following path `~/.sagemaker/config.yaml` and add the following configuration\. The directory that you specify as `container_root` must already exist\. The SageMaker Python SDK will not try to create it\.

  ```
  local:
  container_root: /home/ec2-user/SageMaker/temp
  ```

  With this configuration, local mode uses the `/temp` directory and not the default `/tmp` directory\.