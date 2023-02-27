# Install Kubeflow Pipelines<a name="kubernetes-sagemaker-components-install"></a>

[Kubeflow Pipelines \(KFP\)](https://www.kubeflow.org/docs/components/pipelines/v2/introduction/) is the pipeline orchestration component of Kubeflow\.

You can deploy Kubeflow Pipelines \(KFP\) on an existing Amazon Elastic Kubernetes Service \(Amazon EKS\) or create a new Amazon EKS cluster\. Use a gateway node to interact with your cluster\. The gateway node can be your local machine or an Amazon EC2 instance\.

The following section guides you through the steps to set up and configure these resources\.

**Topics**
+ [Choose an installation option](#choose-install-option)
+ [Configure your pipeline permissions to access SageMaker](#configure-permissions-for-pipeline)
+ [Access the KFP UI \(Kubeflow Dashboard\)](#access-the-kfp-ui)

## Choose an installation option<a name="choose-install-option"></a>

Kubeflow Pipelines is available as a core component of the full distribution of Kubeflow on AWS or as a standalone installation\.

Select the option that applies to your use case:

1. [Full Kubeflow on AWS Deployment](#full-kubeflow-deployment)

   To use other Kubeflow components in addition to Kubeflow Pipelines, choose the full [AWS distribution of Kubeflow](https://awslabs.github.io/kubeflow-manifests) deployment\. 

1. [Standalone Kubeflow Pipelines Deployment](#kubeflow-pipelines-standalone)

   To use the Kubeflow Pipelines without the other components of Kubeflow, install Kubeflow pipelines standalone\. 

### Full Kubeflow on AWS Deployment<a name="full-kubeflow-deployment"></a>

To install the full release of Kubeflow on AWS, choose the vanilla deployment option from [Kubeflow on AWS deployment guide](https://awslabs.github.io/kubeflow-manifests/docs/deployment/) or any other deployment option supporting integrations with various AWS services \(Amazon S3, Amazon RDS, Amazon Cognito\)\.

### Standalone Kubeflow Pipelines Deployment<a name="kubeflow-pipelines-standalone"></a>

This section assumes that your user has permissions to create roles and define policies for the role\.

#### Set up a gateway node<a name="set-up-a-gateway-node"></a>

You can use your local machine or an Amazon EC2 instance as your gateway node\. A gateway node is used to create an Amazon EKS cluster and access the Kubeflow Pipelines UI\. 

Complete the following steps to set up your node\. 

1. 

**Create a gateway node\.**

   You can use an existing Amazon EC2 instance or create a new instance with the latest Ubuntu 18\.04 DLAMI version using the steps in  [Launching and Configuring a DLAMI](https://docs.aws.amazon.com/dlami/latest/devguide/launch-config.html)\.

1. 

**Create an IAM role to grant your gateway node access to AWS resources\.**

   Create an IAM role with permissions to the following resources: CloudWatch, AWS CloudFormation, IAM, Amazon EC2, Amazon S3, Amazon EKS\.

   Attach the following policies to the IAM role:
   + CloudWatchLogsFullAccess 
   + [AWSCloudFormationFullAccess](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess)
   + [https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess)
   + IAMFullAccess 
   + AmazonS3FullAccess 
   + AmazonEC2FullAccess 
   + AmazonEKSAdminPolicy \(Create this policy using the schema from [Amazon EKS Identity\-Based Policy Examples](https://docs.aws.amazon.com/eks/latest/userguide/security_iam_id-based-policy-examples.html)\) 

   For information on adding IAM permissions to an IAM role, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.

1. 

**Install the following tools and clients**

   Install and configure the following tools and resources on your gateway node to access the Amazon EKS cluster and KFP User Interface \(UI\)\. 
   + [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html): The command line tool for working with AWS services\. For AWS CLI configuration information, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)\. 
   + [aws\-iam\-authenticator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html) version 0\.1\.31 and above: A tool to use AWS IAM credentials to authenticate to a Kubernetes cluster\.
   + [https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) version above 0\.15: The command line tool for working with Amazon EKS clusters\.
   + [https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl): The command line tool for working with Kubernetes clusters\. The version needs to match your Kubernetes version within one minor version\.
   + [https://console.aws.amazon.com/https://aws.amazon.com/sdk-for-python/](https://console.aws.amazon.com/https://aws.amazon.com/sdk-for-python/)\.

     ```
     pip install boto3
     ```

#### Set up an Amazon EKS cluster<a name="set-up-anamazon-eks-cluster"></a>

1. If you do not have an existing Amazon EKS cluster, run the following steps from the command line of your gateway node, skip this step otherwise\.

   1. Run the following command to create an Amazon EKS cluster with version 1\.17 or above\. Replace `<clustername>` with any name for your cluster\. 

      ```
      eksctl create cluster --name <clustername> --region us-east-1 --auto-kubeconfig --timeout=50m --managed --nodes=1
      ```

   1. When the cluster creation is complete, ensure that you have access to your cluster by listing the cluster's nodes\. 

      ```
      kubectl get nodes
      ```

1. Ensure that the current `kubectl` context points to your cluster with the following command\. The current context is marked with an asterisk \(\*\) in the output\.

   ```
   kubectl config get-contexts
   
   CURRENT NAME     CLUSTER
   *   <username>@<clustername>.us-east-1.eksctl.io   <clustername>.us-east-1.eksctl.io
   ```

1. If the desired cluster is not configured as your current default, update the default with the following command\. 

   ```
   aws eks update-kubeconfig --name <clustername> --region us-east-1
   ```

#### Install Kubeflow Pipelines<a name="install-kubeflow-pipelines"></a>

Run the following steps from the terminal of your gateway node to install Kubeflow Pipelines on your cluster\.

1. Install all [cert\-manager components](https://cert-manager.io/docs/installation/kubectl/)\.

   ```
   kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml
   ```

1. Install the Kubeflow Pipelines\.

   ```
   export PIPELINE_VERSION=2.0.0-alpha.5
   kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/cert-manager/cluster-scoped-resources?ref=$KFP_VERSION"
   kubectl wait --for condition=established --timeout=60s crd/applications.app.k8s.io
   kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/cert-manager/dev?ref=$KFP_VERSION"
   ```

1. Ensure that the Kubeflow Pipelines service and other related resources are running\.

   ```
   kubectl -n kubeflow get all | grep pipeline
   ```

   Your output should look like the following\.

   ```
   pod/ml-pipeline-6b88c67994-kdtjv                      1/1     Running            0          2d
   pod/ml-pipeline-persistenceagent-64d74dfdbf-66stk     1/1     Running            0          2d
   pod/ml-pipeline-scheduledworkflow-65bdf46db7-5x9qj    1/1     Running            0          2d
   pod/ml-pipeline-ui-66cc4cffb6-cmsdb                   1/1     Running            0          2d
   pod/ml-pipeline-viewer-crd-6db65ccc4-wqlzj            1/1     Running            0          2d
   pod/ml-pipeline-visualizationserver-9c47576f4-bqmx4   1/1     Running            0          2d
   service/ml-pipeline                       ClusterIP   10.100.170.170   <none>        8888/TCP,8887/TCP   2d
   service/ml-pipeline-ui                    ClusterIP   10.100.38.71     <none>        80/TCP              2d
   service/ml-pipeline-visualizationserver   ClusterIP   10.100.61.47     <none>        8888/TCP            2d
   deployment.apps/ml-pipeline                       1/1     1            1           2d
   deployment.apps/ml-pipeline-persistenceagent      1/1     1            1           2d
   deployment.apps/ml-pipeline-scheduledworkflow     1/1     1            1           2d
   deployment.apps/ml-pipeline-ui                    1/1     1            1           2d
   deployment.apps/ml-pipeline-viewer-crd            1/1     1            1           2d
   deployment.apps/ml-pipeline-visualizationserver   1/1     1            1           2d
   replicaset.apps/ml-pipeline-6b88c67994                      1         1         1       2d
   replicaset.apps/ml-pipeline-persistenceagent-64d74dfdbf     1         1         1       2d
   replicaset.apps/ml-pipeline-scheduledworkflow-65bdf46db7    1         1         1       2d
   replicaset.apps/ml-pipeline-ui-66cc4cffb6                   1         1         1       2d
   replicaset.apps/ml-pipeline-viewer-crd-6db65ccc4            1         1         1       2d
   replicaset.apps/ml-pipeline-visualizationserver-9c47576f4   1         1         1       2d
   ```

## Configure your pipeline permissions to access SageMaker<a name="configure-permissions-for-pipeline"></a>

In this section, you create an IAM execution role granting Kubeflow Pipeline pods access to SageMaker services\. 

### Configuration for SageMaker components version 2<a name="permissions-for-SM-v2"></a>

 To run SageMaker Components version 2 for Kubeflow Pipelines, you need to install [SageMaker Operator for Kubernetes](https://github.com/aws-controllers-k8s/sagemaker-controller) and configure Role\-Based Access Control \(RBAC\) allowing the Kubeflow Pipelines pods to create SageMaker custom resources in your Kubernetes cluster\.  

**Important**  
Follow this section if you are using Kubeflow pipelines standalone deployment\. If you are using AWS distribution of Kubeflow version 1\.6\.0\-aws\-b1\.0\.0 or above, SageMaker components version 2 are already set up\.

1.  Install SageMaker Operator for Kubernetes to use SageMaker components version 2\. 

   Follow the *Setup* section of [Machine Learning with ACK SageMaker Controller tutorial](https://aws-controllers-k8s.github.io/community/docs/tutorials/sagemaker-example/#setup)\.

1. Configure RBAC permissions for the execution role \(service account\) used by Kubeflow Pipelines pods\. In Kubeflow Pipelines standalone deployment, pipeline runs are executed in the namespace `kubeflow` using the `pipeline-runner` service account\. 

   1. Create a [RoleBinding](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#rolebinding-example) that gives the service account permission to manage SageMaker custom resources\.

      ```
      cat > manage_sagemaker_cr.yaml <<EOF
      apiVersion: rbac.authorization.k8s.io/v1 
      kind: RoleBinding 
      metadata:   
      name: manage-sagemaker-cr   
      namespace: kubeflow 
      subjects: 
      - kind: ServiceAccount 
      name: pipeline-runner   
      namespace: kubeflow 
      roleRef:   
      kind: ClusterRole 
      name: ack-sagemaker-controller   
      apiGroup: rbac.authorization.k8s.io
      EOF
      ```

      ```
      kubectl apply -f manage_sagemaker_cr.yaml
      ```

   1. Ensure that the rolebinding was created by running:

      ```
      kubectl get rolebinding manage-sagemaker-cr -n kubeflow -o yaml
      ```

### Configuration for SageMaker components version 1<a name="permissions-for-SM-v1"></a>

To run SageMaker Components version 1 for Kubeflow Pipelines, the Kubeflow Pipeline pods need access to SageMaker\.

**Important**  
Follow this section whether you are using the full Kubeflow on AWS deployment or Kubeflow Pilepines standalone\. 

To create an IAM execution role granting Kubeflow pipeline pods access to SageMaker, follow those steps:

1. Export your cluster name \(e\.g\., *my\-cluster\-name*\) and cluster region \(e\.g\., *us\-east\-1*\)\.

   ```
   export CLUSTER_NAME=my-cluster-name 
   export CLUSTER_REGION=us-east-1
   ```

1. Export the namespace and service account name according to your installation\.
   +  For the full Kubeflow on AWS installation, export your profile `namespace` \(e\.g\., *kubeflow\-user\-example\-com*\) and *default\-editor* as the service account\. 

     ```
     export NAMESPACE=kubeflow-user-example-com 
     export KUBEFLOW_PIPELINE_POD_SERVICE_ACCOUNT=default-editor
     ```
   + For the standalone Pipelines deployment, export *kubeflow* as the `namespace` and *pipeline\-runner* as the service account\. 

     ```
     export NAMESPACE=kubeflow 
     export KUBEFLOW_PIPELINE_POD_SERVICE_ACCOUNT=pipeline-runner 
     ```

1. Create an [ IAM OIDC provider for the Amazon EKS cluster](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html) with the following command\.

   ```
   eksctl utils associate-iam-oidc-provider --cluster ${CLUSTER_NAME} \ 
               --region ${CLUSTER_REGION} --approve
   ```

1. Create an IAM execution role for the KFP pods to access AWS services \(SageMaker, CloudWatch\)\.

   ```
   eksctl create iamserviceaccount \  
   --name ${KUBEFLOW_PIPELINE_POD_SERVICE_ACCOUNT} \  
   --namespace ${NAMESPACE} --cluster ${CLUSTER_NAME} \ 
   --region ${CLUSTER_REGION} \
   --attach-policy-arn arn:aws:iam::aws:policy/AmazonSageMakerFullAccess \ 
   --attach-policy-arn arn:aws:iam::aws:policy/CloudWatchLogsFullAccess \ 
   --override-existing-serviceaccounts \
   --approve
   ```

Once your pipeline permissions are configured to access SageMaker Components version 1, follow the SageMaker components for Kubeflow pipelines guide on the  [ Kubeflow on AWS documentation](https://awslabs.github.io/kubeflow-manifests/docs/amazon-sagemaker-integration/sagemaker-components-for-kubeflow-pipelines/)\.  

## Access the KFP UI \(Kubeflow Dashboard\)<a name="access-the-kfp-ui"></a>

The Kubeflow Pipelines UI is used for managing and tracking experiments, jobs, and runs on your cluster\. For instructions on how to access the Kubeflow Pipelines UI from your gateway node, follow the steps that apply to your deployment option in this section\.

### Full Kubeflow on AWS Deployment<a name="access-kfp-ui-full-kubeflow-deployment"></a>

Follow the instructions on the [Kubeflow on AWS website](https://awslabs.github.io/kubeflow-manifests/docs/deployment/connect-kubeflow-dashboard/) to connect to the Kubeflow dashboard and navigate to the pipelines tab\.

### Standalone Kubeflow Pipelines Deployment<a name="access-kfp-ui-standalone-kubeflow-pipelines-deployment"></a>

Use port forwarding to access the Kubeflow Pipelines UI from your gateway node by following those steps\.

#### Set up port forwarding to the KFP UI service<a name="set-up-port-forwarding-to-the-kfp-ui-service"></a>

Run the following command from the command line of your gateway node\.

1. Verify that the KFP UI service is running using the following command\.

   ```
   kubectl -n kubeflow get service ml-pipeline-ui
   
   NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
   ml-pipeline-ui   ClusterIP   10.100.38.71   <none>        80/TCP    2d22h
   ```

1. Run the following command to set up port forwarding to the KFP UI service\. This forwards the KFP UI to port 8080 on your gateway node and allows you to access the KFP UI from your browser\. 

   ```
   kubectl port-forward -n kubeflow service/ml-pipeline-ui 8080:80
   ```

   The port forward from your remote machine drops if there is no activity\. Run this command again if your dashboard is unable to get logs or updates\. If the commands return an error, ensure that there is no process already running on the port you are trying to use\. 

#### Access the KFP UI service<a name="set-up-port-forwarding-to-the-kfp-ui-service-access"></a>

Your method of accessing the KFP UI depends on your gateway node type\.
+ Local machine as the gateway node:

  1. Access the dashboard in your browser as follows: 

     ```
     http://localhost:8080
     ```

  1. Choose **Pipelines** to access the pipelines UI\. 
+ Amazon EC2 instance as the gateway node:

  1. You need to set up an SSH tunnel on your Amazon EC2 instance to access the Kubeflow dashboard from your local machine’s browser\. 

     From a new terminal session in your local machine, run the following\. Replace `<public-DNS-of-gateway-node>` with the IP address of your instance found on the Amazon EC2 console\. You can also use the public DNS\. Replace `<path_to_key>` with the path to the pem key used to access the gateway node\. 

     ```
     public_DNS_address=<public-DNS-of-gateway-node>
     key=<path_to_key>
     
     on Ubuntu:
     ssh -i ${key} -L 9000:localhost:8080 ubuntu@${public_DNS_address}
     
     or on Amazon Linux:
     ssh -i ${key} -L 9000:localhost:8080 ec2-user@${public_DNS_address}
     ```

  1. Access the dashboard in your browser\. 

     ```
     http://localhost:9000
     ```

  1. Choose **Pipelines** to access the KFP UI\. 

#### \(Optional\) Grant SageMaker notebook instances access to Amazon EKS, and run KFP pipelines from your notebook\.<a name="add-access-to-additional-iam-users-or-roles"></a>

A SageMaker notebook instance is a fully managed Amazon EC2 compute instance that runs the Jupyter Notebook App\. You can use a notebook instance to create and manage Jupyter notebooks then define, compile, deploy, and run your KFP pipelines using AWS SDK for Python \(Boto3\) or the KFP CLI\. 

1. Follow the steps in [Create a SageMaker Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-setup-working-env.html) to create your notebook instance, then attach the `S3FullAccess` policy to its IAM execution role\.

1. From the command line of your gateway node, run the following command to retrieve the IAM role ARN of the notebook instance you created\. Replace `<instance-name>` with the name of your instance\.

   ```
   aws sagemaker describe-notebook-instance --notebook-instance-name <instance-name> --region <region> --output text --query 'RoleArn'
   ```

   This command outputs the IAM role ARN in the `arn:aws:iam::<account-id>:role/<role-name>` format\. Take note of this ARN\.

1. Run this command to attach the following policies \(AmazonSageMakerFullAccess, AmazonEKSWorkerNodePolicy, AmazonS3FullAccess\) to this IAM role\. Replace `<role-name>` with the `<role-name>` in your ARN\. 

   ```
   aws iam attach-role-policy --role-name <role-name> --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerFullAccess
   aws iam attach-role-policy --role-name <role-name> --policy-arn arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy
   aws iam attach-role-policy --role-name <role-name> --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
   ```

1. Amazon EKS clusters use IAM roles to control access to the cluster\. The rules are implemented in a config map named `aws-auth`\. `eksctl` provides commands to read and edit the `aws-auth` config map\. Only the users that have access to the cluster can edit this config map\.

   `system:masters` is one of the default user groups with super user permissions to the cluster\. Add your user to this group or create a group with more restrictive permissions\.

1. Bind the role to your cluster by running the following command\. Replace `<IAM-Role-arn>` with the ARN of the IAM role\. `<your_username>` can be any unique username\.

   ```
   eksctl create iamidentitymapping \
   --cluster <cluster-name> \
   --arn <IAM-Role-arn> \
   --group system:masters \
   --username <your-username> \
   --region <region>
   ```

1. Open a Jupyter notebook on your SageMaker instance and run the following command to ensure that it has access to the cluster\.

   ```
   aws eks --region <region> update-kubeconfig --name <cluster-name>
   kubectl -n kubeflow get all | grep pipeline
   ```