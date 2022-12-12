# Install Kubeflow Pipelines<a name="setup"></a>

To use Kubeflow Pipelines \(KFP\), you need an Amazon Elastic Kubernetes Service \(Amazon EKS\) cluster and a gateway node to interact with that cluster\. The following sections show the steps needed to set up these resources\.

**Topics**
+ [Choose an installation option](#choose-install-option)
+ [Configure your pipeline permissions to access SageMaker](#configure-permissions-for-pipeline)
+ [Access the KFP UI](#access-the-kfp-ui)

## Choose an installation option<a name="choose-install-option"></a>

Kubeflow Pipelines offer two installation options\. Select the option that applies to your use case: 

1. [Full Kubeflow on AWS Deployment](#full-kubeflow-deployment)

   To use other Kubeflow components in addition to Kubeflow Pipelines, choose the full [AWS Distribution of Kubeflow](https://awslabs.github.io/kubeflow-manifests) deployment\. 

1. [Standalone Kubeflow Pipelines Deployment](#kubeflow-pipelines-standalone)

   To use the Kubeflow Pipelines without the other components of Kubeflow, install Kubeflow pipelines standalone\. 

### Full Kubeflow on AWS Deployment<a name="full-kubeflow-deployment"></a>

To install the full release of Kubeflow on AWS, [follow one of the deployment guides](https://awslabs.github.io/kubeflow-manifests/docs/deployment/)

### Standalone Kubeflow Pipelines Deployment<a name="kubeflow-pipelines-standalone"></a>

#### Set up a gateway node<a name="set-up-a-gateway-node"></a>

A gateway node is used to create an Amazon EKS cluster and access the Kubeflow Pipelines UI\. Use your local machine or an Amazon EC2 instance as your gateway node\. If you want to use a new Amazon EC2 instance, create one with the latest Ubuntu 18\.04 DLAMI version from the AWS console using the steps in [Launching and Configuring a DLAMI](https://docs.aws.amazon.com/dlami/latest/devguide/launch-config.html)\. 

Complete the following steps to set up your gateway node\. Depending on your environment, you may have certain requirements already configured\. 

1. If you don’t have an existing Amazon EKS cluster, create a user named `your_credentials` using the steps in [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)\. If you have an existing Amazon EKS cluster, use the credentials of the IAM role or user that has access to it\. 

1. Add the following permissions to your user using the steps in [Changing Permissions for an IAM User:](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) 
   + CloudWatchLogsFullAccess 
   + [AWSCloudFormationFullAccess](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess)
   + [https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCloudFormationFullAccess)
   + IAMFullAccess 
   + AmazonS3FullAccess 
   + AmazonEC2FullAccess 
   + AmazonEKSAdminPolicy \(Create this policy using the schema from [Amazon EKS Identity\-Based Policy Examples](https://docs.aws.amazon.com/eks/latest/userguide/security_iam_id-based-policy-examples.html)\) 

1. Install the following on your gateway node to access the Amazon EKS cluster and KFP UI\. 
   + [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)\. If you are using an IAM user, configure your [Access Key ID, Secret Access Key](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) and preferred AWS Region by running: `aws configure` 
   + [aws\-iam\-authenticator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html) version 0\.1\.31 and above 
   + `eksctl` version above 0\.15\. 
   + [https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl) \(The version needs to match your Kubernetes version within one minor version\) 

1. Install `boto3`\. 

   ```
   pip install boto3
   ```

#### Set up an Amazon EKS cluster<a name="set-up-anamazon-eks-cluster"></a>

Run the following steps from the command line of your gateway node to set up an Amazon EKS cluster: 

1. If you do not have an existing Amazon EKS cluster, complete the following substeps\. If you already have an Amazon EKS cluster, skip this step\. 

   1. Run the following from your command line to create an Amazon EKS cluster with version 1\.17 or above\. Replace `<your-cluster-name>` with any name for your cluster\. 

      ```
      eksctl create cluster --name <your-cluster-name> --region us-east-1 --auto-kubeconfig --timeout=50m --managed --nodes=1
      ```

   1. When cluster creation is complete, verify that you have access to the cluster using the following command\. 

      ```
      kubectl get nodes
      ```

1. Verify that the current `kubectl` context is the cluster you want to use with the following command\. The current context is marked with an asterisk \(\*\) in the output\. 

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

1. Install all [cert\-manager components](https://cert-manager.io/docs/installation/kubectl/):

   ```
   kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml
   ```

1. Install the Kubeflow pipelines:

   ```
   export PIPELINE_VERSION=2.0.0-alpha.5
   kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/cert-manager/cluster-scoped-resources?ref=$KFP_VERSION"
   kubectl wait --for condition=established --timeout=60s crd/applications.app.k8s.io
   kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/cert-manager/dev?ref=$KFP_VERSION"
   ```

1. Verify that the Kubeflow Pipelines service and other related resources are running\.

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

In this section, you create IAM users/roles and permissions to be used by Kubeflow Pipeline pods to access SageMaker services\. 

### Configuration for SageMaker components version 2<a name="permissions-for-SM-v2"></a>

 To run SageMaker Components version 2 for Kubeflow Pipelines, you need to install the SageMaker Operator for Kubernetes and configure RBAC permissions for the Kubeflow Pipeline pods to create SageMaker custom resources in your Kubernetes cluster from the pipeline pods\.  

**Important**  
Follow this section if you are using Kubeflow pipelines standalone deployment\. These steps are preconfigured if you are using AWS distribution of Kubeflow version 1\.6\.0\-aws\-b1\.0\.0 or above\. 

1.  Install SageMaker Operator for Kubernetes to use SageMaker components version 2\. 

   Follow the *Setup* section of [Machine Learning with ACK SageMaker Controller tutorial](https://aws-controllers-k8s.github.io/community/docs/tutorials/sagemaker-example/#setup) to install the SageMaker Controller\.

1. Configure RBAC permissions for the service account used by Kubeflow pipeline pods\. In Kubeflow pipeline standalone deployment, the pipeline runs are executed in the namespace `kubeflow` using the pipeline\-runner service account\. 

   1. Create a [RoleBinding](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#rolebinding-example) that grants service account access to manage sagemaker custom resources\.

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

   1. Make sure that the rolebinding was created by running:

      ```
      kubectl get rolebinding manage-sagemaker-cr -n kubeflow -o yaml
      ```

### Configuration for SageMaker components version 1<a name="permissions-for-SM-v1"></a>

To run SageMaker Components version 1 for Kubeflow Pipelines, the Kubeflow Pipeline pods need access to SageMaker\.

**Important**  
Follow this section if you are using full Kubeflow or standalone deployment\. 

To grant SageMaker access to the service account used by Kubeflow pipeline pods, follow those steps:

1. Export your cluster name \(e\.g\., *my\-cluster\-name*\) and cluster region \(e\.g\., *us\-east\-1*\)\.

   ```
   export CLUSTER_NAME=my-cluster-name export CLUSTER_REGION=us-east-1
   ```

1. Export the namespace and service account name according to your installation\.
   +  For the full Kubeflow on AWS installation, export your profile `namespace` \(e\.g\., *kubeflow\-user\-example\-com*\) and *default\-editor* as the service account\. 

     ```
     export NAMESPACE=kubeflow-user-example-com export KUBEFLOW_PIPELINE_POD_SERVICE_ACCOUNT=default-editor
     ```
   + For the standalone Pipelines deployment, export *kubeflow* as the `namespace` and *pipeline\-runner* as the service account\. 

     ```
     export NAMESPACE=kubeflow export KUBEFLOW_PIPELINE_POD_SERVICE_ACCOUNT=pipeline-runner 
     ```

1. Enable OIDC support on the Amazon EKS cluster with the following command\.

   ```
   eksctl utils associate-iam-oidc-provider --cluster ${CLUSTER_NAME} \ 
                 --region ${CLUSTER_REGION} --approve
   ```

1. Create a KFP execution role for the pods to access AWS services\.

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

Once you are finished configuring your pipeline permissions to access SageMaker Components version 1, follow the SageMaker components for Kubeflow pipelines guide on the  [Kubeflow on AWS documentation](https://awslabs.github.io/kubeflow-manifests/docs/amazon-sagemaker-integration/sagemaker-components-for-kubeflow-pipelines/)\.  

## Access the KFP UI<a name="access-the-kfp-ui"></a>

The Kubeflow Pipelines UI is used for managing and tracking experiments, jobs, and runs on your cluster\. For instructions on how to access the Kubeflow Pipelines UI from your gateway node, follow the steps that apply to your deployment option in this section\.

### Full Kubeflow on AWS Deployment<a name="access-kfp-ui-full-kubeflow-deployment"></a>

Follow the instructions on the [Kubeflow on AWS website](https://awslabs.github.io/kubeflow-manifests/docs/deployment/connect-kubeflow-dashboard/) to connect to the Kubeflow dashboard and navigate to the pipelines tab\.

### Standalone Kubeflow Pipelines Deployment<a name="access-kfp-ui-standalone-kubeflow-pipelines-deployment"></a>

Use port forwarding to access the Kubeflow Pipelines UI from your gateway node by following those steps\.

#### Set up port forwarding to the KFP UI service<a name="set-up-port-forwarding-to-the-kfp-ui-service"></a>

Run the following from the command line of your gateway node: 

1. Verify that the KFP UI service is running using the following command: 

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
+ Local machine as the gateway node 

  1. Access the dashboard in your browser as follows: 

     ```
     http://localhost:8080
     ```

  1. Choose **Pipelines** to access the pipelines UI\. 
+  Amazon EC2 instance as the gateway node 

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

#### \(Optional\) Add access to additional IAM users or roles<a name="add-access-to-additional-iam-users-or-roles"></a>

If you use an intuitive IDE like Jupyter or want other people in your organization to use the cluster you set up, you can also give them access\. The following steps run through this workflow using SageMaker notebooks\. An SageMaker notebook instance is a fully managed Amazon EC2 compute instance that runs the Jupyter Notebook App\. You use the notebook instance to create and manage Jupyter notebooks to create ML workflows\. You can define, compile, deploy, and run your pipeline using the KFP AWS SDK for Python \(Boto3\) SDK or CLI\. If you’re not using an SageMaker notebook to run Jupyter, you need to install the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) and the latest version of [https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl)\. 

1. Follow the steps in [Create an SageMaker Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-setup-working-env.html) to create a SageMaker notebook instance if you do not already have one\. Give the IAM role for this instance the `S3FullAccess` permission\. 

1. Amazon EKS clusters use IAM users and roles to control access to the cluster\. The rules are implemented in a config map named `aws-auth`\. Only the user/role that has access to the cluster will be able to edit this config map\. Run the following from the command line of your gateway node to get the IAM role of the notebook instance you created\. Replace `<instance-name>` with the name of your instance\. 

   ```
   aws sagemaker describe-notebook-instance --notebook-instance-name <instance-name> --region <region> --output text --query 'RoleArn'
   ```

   This command outputs the IAM role ARN in the `arn:aws:iam::<account-id>:role/<role-name>` format\. Take note of this ARN\. 

1. Run the following to attach the policies the IAM role\. Replace `<role-name>` with the `<role-name>` in your ARN\. 

   ```
   aws iam attach-role-policy --role-name <role-name> --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerFullAccess
   aws iam attach-role-policy --role-name <role-name> --policy-arn arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy
   aws iam attach-role-policy --role-name <role-name> --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
   ```

1. `eksctl` provides commands to read and edit the `aws-auth` config map\. `system:masters` is one of the default user groups\. You add the user to this group\. The `system:masters` group has super user permissions to the cluster\. You can also create a group with more restrictive permissions or you can bind permissions directly to users\. Replace `<IAM-Role-arn>` with the ARN of the IAM role\. `<your_username>` can be any unique username\. 

   ```
   eksctl create iamidentitymapping \
       --cluster <cluster-name> \
       --arn <IAM-Role-arn> \
       --group system:masters \
       --username <your-username> \
       --region <region>
   ```

1. Open the Jupyter notebook on your SageMaker instance and run the following to verify that it has access to the cluster\. 

   ```
   aws eks --region <region> update-kubeconfig --name <cluster-name>
   kubectl -n kubeflow get all | grep pipeline
   ```