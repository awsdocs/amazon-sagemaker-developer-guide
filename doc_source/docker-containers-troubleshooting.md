# Troubleshooting your Docker containers<a name="docker-containers-troubleshooting"></a>

The following are common errors that you might run into when using Docker containers with SageMaker\. Each error is followed by a solution to the error\. 
+ ** Error: SageMaker has lost the Docker daemon\.**

   To fix this error, restart Docker using the following command\.

  ```
  sudo service docker restart
  ```