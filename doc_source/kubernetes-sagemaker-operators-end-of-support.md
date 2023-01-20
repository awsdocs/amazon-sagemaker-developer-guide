# Old SageMaker Operators for Kubernetes<a name="kubernetes-sagemaker-operators-end-of-support"></a>

This section is based on the original version of [SageMaker Operators for Kubernetes](https://github.com/aws/amazon-sagemaker-operator-for-k8s)\.

**Important**  
We are stopping the development and technical support of [SageMaker Operators for Kubernetes](https://github.com/aws/amazon-sagemaker-operator-for-k8s/tree/master) in its original version\. If you are currently using version `v1.2.2` or below of the original version of SageMaker Operators for Kubernetes, we recommend migrating your resources to the latest SageMaker Operators for Kubernetes, the [ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller) based on [AWS Controllers for Kubernetes \(ACK\)](https://aws-controllers-k8s.github.io/community/ )\.  
For information about the migration steps, see [Migrate resources to the latest Operators](kubernetes-sagemaker-operators-migrate.md)\. For answers to frequently asked questions regarding the end of support of the original version of SageMaker Operators for Kubernetes, see [Announcing the End of Support of the Original Version of SageMaker Operator for Kubernetes](kubernetes-sagemaker-operators-eos-announcement.md)

**Topics**
+ [Install SageMaker Operators for Kubernetes](#kubernetes-sagemaker-operators-eos-install)
+ [Use Amazon SageMaker Jobs](kubernetes-sagemaker-jobs.md)
+ [Migrate resources to the latest Operators](kubernetes-sagemaker-operators-migrate.md)
+ [Announcing the End of Support of the Original Version of SageMaker Operator for Kubernetes](kubernetes-sagemaker-operators-eos-announcement.md)

## Install SageMaker Operators for Kubernetes<a name="kubernetes-sagemaker-operators-eos-install"></a>

Use the following steps to install and use SageMaker Operators for Kubernetes to train, tune, and deploy machine learning models with Amazon SageMaker\.

**Topics**
+ [IAM role\-based setup and operator deployment](#iam-role-based-setup-and-operator-deployment)
+ [Clean up resources](#cleanup-operator-resources)
+ [Delete operators](#delete-operators)
+ [Troubleshooting](#troubleshooting)
+ [Images and SMlogs in each Region](#images-and-smlogs-in-each-region)

### IAM role\-based setup and operator deployment<a name="iam-role-based-setup-and-operator-deployment"></a>

The following sections describe the steps to set up and deploy the original version of the operator\.

**Warning**  
**Reminder:** The following steps do not install the latest version of SageMaker Operators for Kubernetes\. To install the new ACK\-based SageMaker Operators for Kubernetes, see [Latest SageMaker Operators for Kubernetes](kubernetes-sagemaker-operators-ack.md)\.

#### Prerequisites<a name="prerequisites"></a>

This guide assumes that you have completed the following prerequisites: 
+ Installed the following tools on the client machine used to access your Kubernetes cluster: 
  + [https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) Version 1\.13 or later\. Use a `kubectl` version that is within one minor version of your Amazon EKS cluster control plane\. For example, a 1\.13 `kubectl` client works with Kubernetes 1\.13 and 1\.14 clusters\. OpenID Connect \(OIDC\) is not supported in versions earlier than 1\.13\. 
  + [https://github.com/weaveworks/eksctl](https://github.com/weaveworks/eksctl) Version 0\.7\.0 or later 
  + [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html) Version 1\.16\.232 or later 
  + \(optional\) [Helm](https://helm.sh/docs/intro/install/) Version 3\.0 or later 
  + [aws\-iam\-authenticator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html) 
+ Have IAM permissions to create roles and attach policies to roles\. 
+ Created a Kubernetes cluster on which to run the operators\. It should either be Kubernetes version 1\.13 or 1\.14\. For automated cluster creation using `eksctl`, see [Getting Started with eksctl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)\. It takes 20 to 30 minutes to provision a cluster\. 

#### Cluster\-scoped deployment<a name="cluster-scoped-deployment"></a>

Before you can deploy your operator using an IAM role, associate an OpenID Connect \(OIDC\) provider with your role to authenticate with the IAM service\. 

##### Create an OpenID Connect Provider for Your Cluster<a name="create-an-openid-connect-provider-for-your-cluster"></a>

The following instructions show how to create and associate an OIDC provider with your Amazon EKS cluster\. 

1. Set the local `CLUSTER_NAME` and `AWS_REGION` environment variables as follows: 

   ```
   # Set the Region and cluster
     export CLUSTER_NAME="<your cluster name>"
     export AWS_REGION="<your region>"
   ```

1. Use the following command to associate the OIDC provider with your cluster\. For more information, see [Enabling IAM Roles for Service Accounts on your Cluster\.](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html) 

   ```
   eksctl utils associate-iam-oidc-provider --cluster ${CLUSTER_NAME} \
         --region ${AWS_REGION} --approve
   ```

   Your output should look like the following: 

   ```
   [_]  eksctl version 0.10.1
     [_]  using region us-east-1
     [_]  IAM OpenID Connect provider is associated with cluster "my-cluster" in "us-east-1"
   ```

Now that the cluster has an OIDC identity provider, you can create a role and give a Kubernetes ServiceAccount permission to assume the role\. 

##### Get the OIDC ID<a name="get-the-oidc-id"></a>

To set up the ServiceAccount, obtain the OpenID Connect issuer URL using the following command: 

```
aws eks describe-cluster --name ${CLUSTER_NAME} --region ${AWS_REGION} \
      --query cluster.identity.oidc.issuer --output text
```

The command returns a URL like the following: 

```
https://oidc.eks.${AWS_REGION}.amazonaws.com/id/D48675832CA65BD10A532F597OIDCID
```

In this URL, the value `D48675832CA65BD10A532F597OIDCID` is the OIDC ID\. The OIDC ID for your cluster is different\. You need this OIDC ID value to create a role\. 

 If your output is `None`, it means that your client version is old\. To work around this, run the following command: 

```
aws eks describe-cluster --region ${AWS_REGION} --query cluster --name ${CLUSTER_NAME} --output text | grep OIDC
```

The OIDC URL is returned as follows: 

```
OIDC https://oidc.eks.us-east-1.amazonaws.com/id/D48675832CA65BD10A532F597OIDCID
```

##### Create an IAM Role<a name="create-an-iam-role"></a>

1. Create a file named `trust.json` and insert the following trust relationship code block into it\. Be sure to replace all `<OIDC ID>`, `<AWS account number>`, and `<EKS Cluster region>` placeholders with values corresponding to your cluster\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "Federated": "arn:aws:iam::<AWS account number>:oidc-provider/oidc.eks.<EKS Cluster region>.amazonaws.com/id/<OIDC ID>"
           },
           "Action": "sts:AssumeRoleWithWebIdentity",
           "Condition": {
             "StringEquals": {
               "oidc.eks.<EKS Cluster region>.amazonaws.com/id/<OIDC ID>:aud": "sts.amazonaws.com",
               "oidc.eks.<EKS Cluster region>.amazonaws.com/id/<OIDC ID>:sub": "system:serviceaccount:sagemaker-k8s-operator-system:sagemaker-k8s-operator-default"
             }
           }
         }
       ]
     }
   ```

1. Run the following command to create a role with the trust relationship defined in `trust.json`\. This role enables the Amazon EKS cluster to get and refresh credentials from IAM\. 

   ```
   aws iam create-role --region ${AWS_REGION} --role-name <role name> --assume-role-policy-document file://trust.json --output=text
   ```

   Your output should look like the following: 

   ```
   ROLE    arn:aws:iam::123456789012:role/my-role 2019-11-22T21:46:10Z    /       ABCDEFSFODNN7EXAMPLE   my-role
     ASSUMEROLEPOLICYDOCUMENT        2012-10-17
     STATEMENT       sts:AssumeRoleWithWebIdentity   Allow
     STRINGEQUALS    sts.amazonaws.com       system:serviceaccount:sagemaker-k8s-operator-system:sagemaker-k8s-operator-default
     PRINCIPAL       arn:aws:iam::123456789012:oidc-provider/oidc.eks.us-east-1.amazonaws.com/id/
   ```

    Take note of `ROLE ARN`; you pass this value to your operator\. 

##### Attach the AmazonSageMakerFullAccess Policy to the Role<a name="attach-the-amazonsagemakerfullaccess-policy-to-the-role"></a>

To give the role access to SageMaker, attach the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy\. If you want to limit permissions to the operator, you can create your own custom policy and attach it\. 

 To attach AmazonSageMakerFullAccess, run the following command: 

```
aws iam attach-role-policy --role-name <role name>  --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerFullAccess
```

The Kubernetes ServiceAccount `sagemaker-k8s-operator-default` should have `AmazonSageMakerFullAccess` permissions\. Confirm this when you install the operator\. 

##### Deploy the Operator<a name="deploy-the-operator"></a>

When deploying your operator, you can use either a YAML file or Helm charts\. 

##### Deploy the Operator Using YAML<a name="deploy-the-operator-using-yaml"></a>

This is the simplest way to deploy your operators\. The process is as follows: 

1. Download the installer script using the following command: 

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/release/rolebased/installer.yaml
   ```

1. Edit the `installer.yaml` file to replace `eks.amazonaws.com/role-arn`\. Replace the ARN here with the Amazon Resource Name \(ARN\) for the OIDC\-based role you’ve created\. 

1. Use the following command to deploy the cluster: 

   ```
   kubectl apply -f installer.yaml
   ```

##### Deploy the Operator Using Helm Charts<a name="deploy-the-operator-using-helm-charts"></a>

Use the provided Helm Chart to install the operator\. 

1. Clone the Helm installer directory using the following command: 

   ```
   git clone https://github.com/aws/amazon-sagemaker-operator-for-k8s.git
   ```

1. Navigate to the `amazon-sagemaker-operator-for-k8s/hack/charts/installer` folder\. Edit the `rolebased/values.yaml` file, which includes high\-level parameters for the chart\. Replace the role ARN here with the Amazon Resource Name \(ARN\) for the OIDC\-based role you’ve created\. 

1. Install the Helm Chart using the following command: 

   ```
   kubectl create namespace sagemaker-k8s-operator-system
     helm install --namespace sagemaker-k8s-operator-system sagemaker-operator rolebased/
   ```

   If you decide to install the operator into a namespace other than the one specified, you need to adjust the namespace defined in the IAM role `trust.json` file to match\. 

1. After a moment, the chart is installed with a randomly generated name\. Verify that the installation succeeded by running the following command: 

   ```
   helm ls
   ```

   Your output should look like the following: 

   ```
   NAME                    NAMESPACE                       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
     sagemaker-operator      sagemaker-k8s-operator-system   1               2019-11-20 23:14:59.6777082 +0000 UTC   deployed        sagemaker-k8s-operator-0.1.0
   ```

##### Verify the operator deployment<a name="verify-the-operator-deployment"></a>

1. You should be able to see the SageMaker Custom Resource Definitions \(CRDs\) for each operator deployed to your cluster by running the following command: 

   ```
   kubectl get crd | grep sagemaker
   ```

   Your output should look like the following: 

   ```
   batchtransformjobs.sagemaker.aws.amazon.com         2019-11-20T17:12:34Z
     endpointconfigs.sagemaker.aws.amazon.com            2019-11-20T17:12:34Z
     hostingdeployments.sagemaker.aws.amazon.com         2019-11-20T17:12:34Z
     hyperparametertuningjobs.sagemaker.aws.amazon.com   2019-11-20T17:12:34Z
     models.sagemaker.aws.amazon.com                     2019-11-20T17:12:34Z
     trainingjobs.sagemaker.aws.amazon.com               2019-11-20T17:12:34Z
   ```

1. Ensure that the operator pod is running successfully\. Use the following command to list all pods: 

   ```
   kubectl -n sagemaker-k8s-operator-system get pods
   ```

   You should see a pod named `sagemaker-k8s-operator-controller-manager-*****` in the namespace `sagemaker-k8s-operator-system` as follows: 

   ```
   NAME                                                         READY   STATUS    RESTARTS   AGE
     sagemaker-k8s-operator-controller-manager-12345678-r8abc     2/2     Running   0          23s
   ```

#### Namespace\-scoped deployment<a name="namespace-scoped-deployment"></a>

You have the option to install your operator within the scope of an individual Kubernetes namespace\. In this mode, the controller only monitors and reconciles resources with SageMaker if the resources are created within that namespace\. This allows for finer\-grained control over which controller is managing which resources\. This is useful for deploying to multiple AWS accounts or controlling which users have access to particular jobs\. 

This guide outlines how to install an operator into a particular, predefined namespace\. To deploy a controller into a second namespace, follow the guide from beginning to end and change out the namespace in each step\. 

##### Create an OpenID Connect Provider for Your Amazon EKS cluster<a name="create-an-openid-connect-provider-for-your-eks-cluster"></a>

The following instructions show how to create and associate an OIDC provider with your Amazon EKS cluster\. 

1. Set the local `CLUSTER_NAME` and `AWS_REGION` environment variables as follows: 

   ```
   # Set the region and cluster
     export CLUSTER_NAME="<your cluster name>"
     export AWS_REGION="<your region>"
   ```

1. Use the following command to associate the OIDC provider with your cluster\. For more information, see [Enabling IAM Roles for Service Accounts on your Cluster\.](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html) 

   ```
   eksctl utils associate-iam-oidc-provider --cluster ${CLUSTER_NAME} \
         --region ${AWS_REGION} --approve
   ```

   Your output should look like the following: 

   ```
   [_]  eksctl version 0.10.1
     [_]  using region us-east-1
     [_]  IAM OpenID Connect provider is associated with cluster "my-cluster" in "us-east-1"
   ```

 Now that the cluster has an OIDC identity provider, create a role and give a Kubernetes ServiceAccount permission to assume the role\. 

##### Get your OIDC ID<a name="get-your-oidc-id"></a>

To set up the ServiceAccount, first obtain the OpenID Connect issuer URL using the following command: 

```
aws eks describe-cluster --name ${CLUSTER_NAME} --region ${AWS_REGION} \
      --query cluster.identity.oidc.issuer --output text
```

The command returns a URL like the following: 

```
https://oidc.eks.${AWS_REGION}.amazonaws.com/id/D48675832CA65BD10A532F597OIDCID
```

In this URL, the value D48675832CA65BD10A532F597OIDCID is the OIDC ID\. The OIDC ID for your cluster will be different\. You need this OIDC ID value to create a role\. 

 If your output is `None`, it means that your client version is old\. To work around this, run the following command: 

```
aws eks describe-cluster --region ${AWS_REGION} --query cluster --name ${CLUSTER_NAME} --output text | grep OIDC
```

The OIDC URL is returned as follows: 

```
OIDC https://oidc.eks.us-east-1.amazonaws.com/id/D48675832CA65BD10A532F597OIDCID
```

##### Create your IAM Role<a name="create-your-iam-role"></a>

1. Create a file named `trust.json` and insert the following trust relationship code block into it\. Be sure to replace all `<OIDC ID>`, `<AWS account number>`, `<EKS Cluster region>`, and `<Namespace>` placeholders with values corresponding to your cluster\. For the purposes of this guide, `my-namespace` is used for the `<Namespace>` value\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "Federated": "arn:aws:iam::<AWS account number>:oidc-provider/oidc.eks.<EKS Cluster region>.amazonaws.com/id/<OIDC ID>"
           },
           "Action": "sts:AssumeRoleWithWebIdentity",
           "Condition": {
             "StringEquals": {
               "oidc.eks.<EKS Cluster region>.amazonaws.com/id/<OIDC ID>:aud": "sts.amazonaws.com",
               "oidc.eks.<EKS Cluster region>.amazonaws.com/id/<OIDC ID>:sub": "system:serviceaccount:<Namespace>:sagemaker-k8s-operator-default"
             }
           }
         }
       ]
     }
   ```

1. Run the following command to create a role with the trust relationship defined in `trust.json`\. This role enables the Amazon EKS cluster to get and refresh credentials from IAM\. 

   ```
   aws iam create-role --region ${AWS_REGION} --role-name <role name> --assume-role-policy-document file://trust.json --output=text
   ```

   Your output should look like the following: 

   ```
   ROLE    arn:aws:iam::123456789012:role/my-role 2019-11-22T21:46:10Z    /       ABCDEFSFODNN7EXAMPLE   my-role
     ASSUMEROLEPOLICYDOCUMENT        2012-10-17
     STATEMENT       sts:AssumeRoleWithWebIdentity   Allow
     STRINGEQUALS    sts.amazonaws.com       system:serviceaccount:my-namespace:sagemaker-k8s-operator-default
     PRINCIPAL       arn:aws:iam::123456789012:oidc-provider/oidc.eks.us-east-1.amazonaws.com/id/
   ```

Take note of `ROLE ARN`\. You pass this value to your operator\. 

##### Attach the AmazonSageMakerFullAccess Policy to your Role<a name="attach-the-amazonsagemakerfullaccess-policy-to-your-role"></a>

To give the role access to SageMaker, attach the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy\. If you want to limit permissions to the operator, you can create your own custom policy and attach it\. 

 To attach AmazonSageMakerFullAccess, run the following command: 

```
aws iam attach-role-policy --role-name <role name>  --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerFullAccess
```

The Kubernetes ServiceAccount `sagemaker-k8s-operator-default` should have `AmazonSageMakerFullAccess` permissions\. Confirm this when you install the operator\. 

##### Deploy the Operator to Your Namespace<a name="deploy-the-operator-to-your-namespace"></a>

When deploying your operator, you can use either a YAML file or Helm charts\. 

##### Deploy the Operator to Your Namespace Using YAML<a name="deploy-the-operator-to-your-namespace-using-yaml"></a>

There are two parts to deploying an operator within the scope of a namespace\. The first is the set of CRDs that are installed at a cluster level\. These resource definitions only need to be installed once per Kubernetes cluster\. The second part is the operator permissions and deployment itself\. 

 If you have not already installed the CRDs into the cluster, apply the CRD installer YAML using the following command: 

```
kubectl apply -f https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/release/rolebased/namespaced/crd.yaml
```

To install the operator onto the cluster: 

1. Download the operator installer YAML using the following command: 

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/release/rolebased/namespaced/operator.yaml
   ```

1. Update the installer YAML to place the resources into your specified namespace using the following command: 

   ```
   sed -i -e 's/PLACEHOLDER-NAMESPACE/<YOUR NAMESPACE>/g' operator.yaml
   ```

1. Edit the `operator.yaml` file to place resources into your `eks.amazonaws.com/role-arn`\. Replace the ARN here with the Amazon Resource Name \(ARN\) for the OIDC\-based role you’ve created\. 

1. Use the following command to deploy the cluster: 

   ```
   kubectl apply -f operator.yaml
   ```

##### Deploy the Operator to Your Namespace Using Helm Charts<a name="deploy-the-operator-to-your-namespace-using-helm-charts"></a>

There are two parts needed to deploy an operator within the scope of a namespace\. The first is the set of CRDs that are installed at a cluster level\. These resource definitions only need to be installed once per Kubernetes cluster\. The second part is the operator permissions and deployment itself\. When using helm charts you have to first create the namespace using `kubectl`\. 

1. Clone the Helm installer directory using the following command: 

   ```
   git clone https://github.com/aws/amazon-sagemaker-operator-for-k8s.git
   ```

1. Navigate to the `amazon-sagemaker-operator-for-k8s/hack/charts/installer/namespaced` folder\. Edit the `rolebased/values.yaml` file, which includes high\-level parameters for the chart\. Replace the role ARN here with the Amazon Resource Name \(ARN\) for the OIDC\-based role you’ve created\. 

1. Install the Helm Chart using the following command: 

   ```
   helm install crds crd_chart/
   ```

1. Create the required namespace and install the operator using the following command: 

   ```
   kubectl create namespace <namespace>
     helm install --n <namespace> op operator_chart/
   ```

1. After a moment, the chart is installed with the name `sagemaker-operator`\. Verify that the installation succeeded by running the following command: 

   ```
   helm ls
   ```

   Your output should look like the following: 

   ```
   NAME                    NAMESPACE                       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
     sagemaker-operator      my-namespace                    1               2019-11-20 23:14:59.6777082 +0000 UTC   deployed        sagemaker-k8s-operator-0.1.0
   ```

##### Verify the operator deployment to your namespace<a name="verify-the-operator-deployment-to-your-namespace"></a>

1. You should be able to see the SageMaker Custom Resource Definitions \(CRDs\) for each operator deployed to your cluster by running the following command: 

   ```
   kubectl get crd | grep sagemaker
   ```

   Your output should look like the following: 

   ```
   batchtransformjobs.sagemaker.aws.amazon.com         2019-11-20T17:12:34Z
     endpointconfigs.sagemaker.aws.amazon.com            2019-11-20T17:12:34Z
     hostingdeployments.sagemaker.aws.amazon.com         2019-11-20T17:12:34Z
     hyperparametertuningjobs.sagemaker.aws.amazon.com   2019-11-20T17:12:34Z
     models.sagemaker.aws.amazon.com                     2019-11-20T17:12:34Z
     trainingjobs.sagemaker.aws.amazon.com               2019-11-20T17:12:34Z
   ```

1. Ensure that the operator pod is running successfully\. Use the following command to list all pods: 

   ```
   kubectl -n my-namespace get pods
   ```

   You should see a pod named `sagemaker-k8s-operator-controller-manager-*****` in the namespace `my-namespace` as follows: 

   ```
   NAME                                                         READY   STATUS    RESTARTS   AGE
     sagemaker-k8s-operator-controller-manager-12345678-r8abc     2/2     Running   0          23s
   ```

#### Install the SageMaker logs `kubectl` plugin<a name="install-the-amazon-sagemaker-logs-kubectl-plugin"></a>

 As part of the SageMaker Operators for Kubernetes, you can use the `smlogs` [plugin](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/) for `kubectl`\. This enables SageMaker CloudWatch logs to be streamed with `kubectl`\. `kubectl` must be installed onto your [PATH](http://www.linfo.org/path_env_var.html)\. The following commands place the binary in the `sagemaker-k8s-bin` directory in your home directory, and add that directory to your `PATH`\. 

```
export os="linux"
  
  wget https://amazon-sagemaker-operator-for-k8s-us-east-1.s3.amazonaws.com/kubectl-smlogs-plugin/v1/${os}.amd64.tar.gz
  tar xvzf ${os}.amd64.tar.gz
  
  # Move binaries to a directory in your homedir.
  mkdir ~/sagemaker-k8s-bin
  cp ./kubectl-smlogs.${os}.amd64/kubectl-smlogs ~/sagemaker-k8s-bin/.
  
  # This line adds the binaries to your PATH in your .bashrc.
  
  echo 'export PATH=$PATH:~/sagemaker-k8s-bin' >> ~/.bashrc
  
  # Source your .bashrc to update environment variables:
  source ~/.bashrc
```

Use the following command to verify that the `kubectl` plugin is installed correctly: 

```
kubectl smlogs
```

If the `kubectl` plugin is installed correctly, your output should look like the following: 

```
View SageMaker logs via Kubernetes
  
  Usage:
    smlogs [command]
  
  Aliases:
    smlogs, SMLogs, Smlogs
  
  Available Commands:
    BatchTransformJob       View BatchTransformJob logs via Kubernetes
    TrainingJob             View TrainingJob logs via Kubernetes
    help                    Help about any command
  
  Flags:
    -h, --help   help for smlogs
  
  Use "smlogs [command] --help" for more information about a command.
```

### Clean up resources<a name="cleanup-operator-resources"></a>

To uninstall the operator from your cluster, you must first make sure to delete all SageMaker resources from the cluster\. Failure to do so causes the operator delete operation to hang\. Run the following commands to stop all jobs: 

```
# Delete all SageMaker jobs from Kubernetes
  kubectl delete --all --all-namespaces hyperparametertuningjob.sagemaker.aws.amazon.com
  kubectl delete --all --all-namespaces trainingjobs.sagemaker.aws.amazon.com
  kubectl delete --all --all-namespaces batchtransformjob.sagemaker.aws.amazon.com
  kubectl delete --all --all-namespaces hostingdeployment.sagemaker.aws.amazon.com
```

You should see output similar to the following: 

```
$ kubectl delete --all --all-namespaces trainingjobs.sagemaker.aws.amazon.com
  trainingjobs.sagemaker.aws.amazon.com "xgboost-mnist-from-for-s3" deleted
  
  $ kubectl delete --all --all-namespaces hyperparametertuningjob.sagemaker.aws.amazon.com
  hyperparametertuningjob.sagemaker.aws.amazon.com "xgboost-mnist-hpo" deleted
  
  $ kubectl delete --all --all-namespaces batchtransformjob.sagemaker.aws.amazon.com
  batchtransformjob.sagemaker.aws.amazon.com "xgboost-mnist" deleted
  
  $ kubectl delete --all --all-namespaces hostingdeployment.sagemaker.aws.amazon.com
  hostingdeployment.sagemaker.aws.amazon.com "host-xgboost" deleted
```

After you delete all SageMaker jobs, see [Delete operators](#delete-operators) to delete the operator from your cluster\.

### Delete operators<a name="delete-operators"></a>

#### Delete cluster\-based operators<a name="delete-cluster-based-operators"></a>

##### Operators installed using YAML<a name="operators-installed-using-yaml"></a>

To uninstall the operator from your cluster, make sure that all SageMaker resources have been deleted from the cluster\. Failure to do so causes the operator delete operation to hang\.

**Note**  
Before deleting your cluster, be sure to delete all SageMaker resources from the cluster\. See [Clean up resources](#cleanup-operator-resources) for more information\.

After you delete all SageMaker jobs, use `kubectl` to delete the operator from the cluster:

```
# Delete the operator and its resources
  kubectl delete -f /installer.yaml
```

You should see output similar to the following: 

```
$ kubectl delete -f raw-yaml/installer.yaml
  namespace "sagemaker-k8s-operator-system" deleted
  customresourcedefinition.apiextensions.k8s.io "batchtransformjobs.sagemaker.aws.amazon.com" deleted
  customresourcedefinition.apiextensions.k8s.io "endpointconfigs.sagemaker.aws.amazon.com" deleted
  customresourcedefinition.apiextensions.k8s.io "hostingdeployments.sagemaker.aws.amazon.com" deleted
  customresourcedefinition.apiextensions.k8s.io "hyperparametertuningjobs.sagemaker.aws.amazon.com" deleted
  customresourcedefinition.apiextensions.k8s.io "models.sagemaker.aws.amazon.com" deleted
  customresourcedefinition.apiextensions.k8s.io "trainingjobs.sagemaker.aws.amazon.com" deleted
  role.rbac.authorization.k8s.io "sagemaker-k8s-operator-leader-election-role" deleted
  clusterrole.rbac.authorization.k8s.io "sagemaker-k8s-operator-manager-role" deleted
  clusterrole.rbac.authorization.k8s.io "sagemaker-k8s-operator-proxy-role" deleted
  rolebinding.rbac.authorization.k8s.io "sagemaker-k8s-operator-leader-election-rolebinding" deleted
  clusterrolebinding.rbac.authorization.k8s.io "sagemaker-k8s-operator-manager-rolebinding" deleted
  clusterrolebinding.rbac.authorization.k8s.io "sagemaker-k8s-operator-proxy-rolebinding" deleted
  service "sagemaker-k8s-operator-controller-manager-metrics-service" deleted
  deployment.apps "sagemaker-k8s-operator-controller-manager" deleted
  secrets "sagemaker-k8s-operator-abcde" deleted
```

##### Operators installed using Helm Charts<a name="operators-installed-using-helm-charts"></a>

To delete the operator CRDs, first delete all the running jobs\. Then delete the Helm Chart that was used to deploy the operators using the following commands: 

```
# get the helm charts
  helm ls
  
  # delete the charts
  helm delete <chart_name>
```

#### Delete namespace\-based operators<a name="delete-namespace-based-operators"></a>

##### Operators installed with YAML<a name="operators-installed-with-yaml"></a>

To uninstall the operator from your cluster, first make sure that all SageMaker resources have been deleted from the cluster\. Failure to do so causes the operator delete operation to hang\.

**Note**  
Before deleting your cluster, be sure to delete all SageMaker resources from the cluster\. See [Clean up resources](#cleanup-operator-resources) for more information\.

After you delete all SageMaker jobs, use `kubectl` to first delete the operator from the namespace and then the CRDs from the cluster\. Run the following commands to delete the operator from the cluster: 

```
# Delete the operator using the same yaml file that was used to install the operator
  kubectl delete -f operator.yaml
  
  # Now delete the CRDs using the CRD installer yaml
  kubectl delete -f https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/release/rolebased/namespaced/crd.yaml
  
  # Now you can delete the namespace if you want
  kubectl delete namespace <namespace>
```

##### Operators installed with Helm Charts<a name="operators-installed-with-helm-charts"></a>

To delete the operator CRDs, first delete all the running jobs\. Then delete the Helm Chart that was used to deploy the operators using the following commands: 

```
# Delete the operator
  helm delete <chart_name>
  
  # delete the crds
  helm delete crds
  
  # optionally delete the namespace
  kubectl delete namespace <namespace>
```

### Troubleshooting<a name="troubleshooting"></a>

#### Debugging a Failed Job<a name="debugging-a-failed-job"></a>
+ Check the job status by running the following: 

  ```
  kubectl get <CRD Type> <job name>
  ```
+ If the job was created in SageMaker, you can use the following command to see the `STATUS` and the `SageMaker Job Name`: 

  ```
  kubectl get <crd type> <job name>
  ```
+ You can use `smlogs` to find the cause of the issue using the following command: 

  ```
  kubectl smlogs <crd type> <job name>
  ```
+  You can also use the `describe` command to get more details about the job using the following command\. The output has an `additional` field that has more information about the status of the job\. 

  ```
  kubectl describe <crd type> <job name>
  ```
+ If the job was not created in SageMaker, then use the logs of the operator’s pod to find the cause of the issue as follows: 

  ```
  $ kubectl get pods -A | grep sagemaker
    # Output:
    sagemaker-k8s-operator-system   sagemaker-k8s-operator-controller-manager-5cd7df4d74-wh22z   2/2     Running   0          3h33m
    
    $ kubectl logs -p <pod name> -c manager -n sagemaker-k8s-operator-system
  ```

#### Deleting an Operator CRD<a name="deleting-an-operator-crd"></a>

If deleting a job is not working, check if the operator is running\. If the operator is not running, then you have to delete the finalizer using the following steps: 

1. In a new terminal, open the job in an editor using `kubectl edit` as follows: 

   ```
   kubectl edit <crd type> <job name>
   ```

1. Edit the job to delete the finalizer by removing the following two lines from the file\. Save the file and the job is be deleted\. 

   ```
   finalizers:
     - sagemaker-operator-finalizer
   ```

### Images and SMlogs in each Region<a name="images-and-smlogs-in-each-region"></a>

The following table lists the available operator images and SMLogs in each region\. 


|  Region  |  Controller Image  |  Linux SMLogs  | 
| --- | --- | --- | 
|  us\-east\-1  |  957583890962\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/amazon\-sagemaker\-operator\-for\-k8s:v1  |  [https://s3\.us\-east\-1\.amazonaws\.com/amazon\-sagemaker\-operator\-for\-k8s\-us\-east\-1/kubectl\-smlogs\-plugin/v1/linux\.amd64\.tar\.gz](https://s3.us-east-1.amazonaws.com/amazon-sagemaker-operator-for-k8s-us-east-1/kubectl-smlogs-plugin/v1/linux.amd64.tar.gz)  | 
|  us\-east\-2  |  922499468684\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/amazon\-sagemaker\-operator\-for\-k8s:v1  |  [https://s3\.us\-east\-2\.amazonaws\.com/amazon\-sagemaker\-operator\-for\-k8s\-us\-east\-2/kubectl\-smlogs\-plugin/v1/linux\.amd64\.tar\.gz](https://s3.us-east-2.amazonaws.com/amazon-sagemaker-operator-for-k8s-us-east-2/kubectl-smlogs-plugin/v1/linux.amd64.tar.gz)  | 
|  us\-west\-2  |  640106867763\.dkr\.ecr\.us\-west\-2\.amazonaws\.com/amazon\-sagemaker\-operator\-for\-k8s:v1  |  [https://s3\.us\-west\-2\.amazonaws\.com/amazon\-sagemaker\-operator\-for\-k8s\-us\-west\-2/kubectl\-smlogs\-plugin/v1/linux\.amd64\.tar\.gz](https://s3.us-west-2.amazonaws.com/amazon-sagemaker-operator-for-k8s-us-west-2/kubectl-smlogs-plugin/v1/linux.amd64.tar.gz)  | 
|  eu\-west\-1  |  613661167059\.dkr\.ecr\.eu\-west\-1\.amazonaws\.com/amazon\-sagemaker\-operator\-for\-k8s:v1  |  [https://s3\.eu\-west\-1\.amazonaws\.com/amazon\-sagemaker\-operator\-for\-k8s\-eu\-west\-1/kubectl\-smlogs\-plugin/v1/linux\.amd64\.tar\.gz](https://s3.eu-west-1.amazonaws.com/amazon-sagemaker-operator-for-k8s-eu-west-1/kubectl-smlogs-plugin/v1/linux.amd64.tar.gz)  | 