# Extend a Pre\-built Container<a name="prebuilt-containers-extend"></a>

If a pre\-built SageMaker container doesn't fulfill all of your requirements, you can extend the existing image to accommodate your needs\. Even if there is direct support for your environment or framework, you may want to add additional functionality or configure your container environment differently\. By extending a pre\-built image, you can leverage the included deep learning libraries and settings without having to create an image from scratch\. You can extend the container to add libraries, modify settings, and install additional dependencies\. 

The following tutorial shows how to extend a pre\-built SageMaker image and publish it to Amazon ECR\.

**Topics**
+ [Requirements to Extend a Pre\-built Container](#prebuilt-containers-extend-required)
+ [Extend SageMaker Containers to Run a Python Script](#prebuilt-containers-extend-tutorial)

## Requirements to Extend a Pre\-built Container<a name="prebuilt-containers-extend-required"></a>

To extend a pre\-built SageMaker image, you need to set the following environment variables within your Dockerfile\. For more information on environment variables with SageMaker containers, see the [SageMaker Training Toolkit GitHub repo](https://github.com/aws/sagemaker-training-toolkit/blob/master/ENVIRONMENT_VARIABLES.md)\.
+ `SAGEMAKER_SUBMIT_DIRECTORY`: The directory within the container in which the Python script for training is located\.
+ `SAGEMAKER_PROGRAM`: The Python script that should be invoked and used as the entry point for training\.

You can also install additional libraries by including the following in your Dockerfile:

```
RUN pip install <library>
```

The following tutorial shows how to use these environment variables\.

## Extend SageMaker Containers to Run a Python Script<a name="prebuilt-containers-extend-tutorial"></a>

In this tutorial, you learn how to extend the SageMaker PyTorch container with a Python file that uses the CIFAR\-10 dataset\. By extending the SageMaker PyTorch container, you utilize the existing training solution made to work with SageMaker\. This tutorial extends a training image, but the same steps can be taken to extend an inference image\. For a full list of the available images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\.

To run your own training model using the SageMaker containers, build a Docker container through a SageMaker Notebook instance\. 

### Step 1: Create an SageMaker Notebook Instance<a name="extend-step1"></a>

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\. 

1. In the left navigation pane, choose **Notebook**, choose **Notebook instances**, and then choose **Create notebook instance**\. 

1. On the **Create notebook instance** page, provide the following information: 

   1. For **Notebook instance name**, enter **RunScriptNotebookInstance**\.

   1. For **Notebook Instance type**, choose **ml\.t2\.medium**\.

   1. In the **Permissions and encryption** section, do the following:

      1. For **IAM role**, choose **Create a new role**\.

      1. On the **Create an IAM role** page, choose **Specific S3 buckets**, specify an Amazon S3 bucket named **sagemaker\-run\-script**, and then choose **Create role**\.

         SageMaker creates an IAM role named `AmazonSageMaker-ExecutionRole-YYYYMMDDTHHmmSS`, such as `AmazonSageMaker-ExecutionRole-20190429T110788`\. Note that the execution role naming convention uses the date and time when the role was created, separated by a `T`\.

   1. For **Root Access**, choose **Enable**\.

   1. Choose **Create notebook instance**\. 

1. On the **Notebook instances** page, the **Status** is **Pending**\. It can take a few minutes for Amazon CloudWatch Internet Monitor to launch a machine learning compute instance—in this case, it launches a notebook instance—and attach an ML storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server and a set of Anaconda libraries\. For more information, see [  CreateNotebookInstance](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html)\. 

   

1. In the **Permissions and encryption** section, copy **the IAM role ARN number**, and paste it into a notepad file to save it temporarily\. You use this IAM role ARN number later to configure a local training estimator in the notebook instance\. **The IAM role ARN number** looks like the following: `'arn:aws:iam::111122223333:role/service-role/AmazonSageMaker-ExecutionRole-20190429T110788'` 

1. After the status of the notebook instance changes to **InService**, choose **Open JupyterLab**\.

### Step 2: Create and Upload the Dockerfile and Python Training Scripts<a name="extend-step2"></a>

1. After JupyterLab opens, create a new folder in the home directory of your JupyterLab\. In the upper\-left corner, choose the **New Folder** icon, and then enter the folder name `docker_test_folder`\. 

1.  Create a `Dockerfile` text file in the `docker_test_folder` directory\. 

   1. Choose the **New Launcher** icon \(\+\) in the upper\-left corner\. 

   1. In the right pane under the **Other** section, choose **Text File**\.

   1.  Paste the following `Dockerfile` sample code into your text file\. 

      ```
      # SageMaker PyTorch image
      FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-training:1.5.1-cpu-py36-ubuntu16.04
      
      ENV PATH="/opt/ml/code:${PATH}"
      
      # this environment variable is used by the SageMaker PyTorch container to determine our user code directory.
      ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code
      
      # /opt/ml and all subdirectories are utilized by SageMaker, use the /code subdirectory to store your user code.
      COPY cifar10.py /opt/ml/code/cifar10.py
      
      # Defines cifar10.py as script entrypoint 
      ENV SAGEMAKER_PROGRAM cifar10.py
      ```

      The Dockerfile script performs the following tasks:
      + `FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-training:1.5.1-cpu-py36-ubuntu16.04` – Downloads the SageMaker PyTorch base image\. You can replace this with any SageMaker base image you want to bring to build containers\.
      + `ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code` – Sets `/opt/ml/code` as the training script directory\.
      + `COPY cifar10.py /opt/ml/code/cifar10.py` – Copies the script to the location inside the container that is expected by SageMaker\. The script must be located in this folder\.
      + `ENV SAGEMAKER_PROGRAM cifar10.py` – Sets your `cifar10.py` training script as the entrypoint script\.

   1.  On the left directory navigation pane, the text file name might automatically be named `untitled.txt`\. To rename the file, right\-click the file, choose **Rename**, rename the file as `Dockerfile` without the `.txt` extension, and then press `Ctrl+s` or `Command+s` to save the file\.

1. Create or upload a training script `cifar10.py` in the `docker_test_folder`\. You can use the following example script for this exercise\. 

   ```
   import ast
   import argparse
   import logging
   
   import os
   
   import torch
   import torch.distributed as dist
   import torch.nn as nn
   import torch.nn.parallel
   import torch.optim
   import torch.utils.data
   import torch.utils.data.distributed
   import torchvision
   import torchvision.models
   import torchvision.transforms as transforms
   import torch.nn.functional as F
   
   logger=logging.getLogger(__name__)
   logger.setLevel(logging.DEBUG)
   
   classes=('plane', 'car', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck')
   
   
   # https://github.com/pytorch/tutorials/blob/master/beginner_source/blitz/cifar10_tutorial.py#L118
   class Net(nn.Module):
       def __init__(self):
           super(Net, self).__init__()
           self.conv1=nn.Conv2d(3, 6, 5)
           self.pool=nn.MaxPool2d(2, 2)
           self.conv2=nn.Conv2d(6, 16, 5)
           self.fc1=nn.Linear(16 * 5 * 5, 120)
           self.fc2=nn.Linear(120, 84)
           self.fc3=nn.Linear(84, 10)
   
       def forward(self, x):
           x=self.pool(F.relu(self.conv1(x)))
           x=self.pool(F.relu(self.conv2(x)))
           x=x.view(-1, 16 * 5 * 5)
           x=F.relu(self.fc1(x))
           x=F.relu(self.fc2(x))
           x=self.fc3(x)
           return x
   
   
   def _train(args):
       is_distributed=len(args.hosts) > 1 and args.dist_backend is not None
       logger.debug("Distributed training - {}".format(is_distributed))
   
       if is_distributed:
           # Initialize the distributed environment.
           world_size=len(args.hosts)
           os.environ['WORLD_SIZE']=str(world_size)
           host_rank=args.hosts.index(args.current_host)
           dist.init_process_group(backend=args.dist_backend, rank=host_rank, world_size=world_size)
           logger.info(
               'Initialized the distributed environment: \'{}\' backend on {} nodes. '.format(
                   args.dist_backend,
                   dist.get_world_size()) + 'Current host rank is {}. Using cuda: {}. Number of gpus: {}'.format(
                   dist.get_rank(), torch.cuda.is_available(), args.num_gpus))
   
       device='cuda' if torch.cuda.is_available() else 'cpu'
       logger.info("Device Type: {}".format(device))
   
       logger.info("Loading Cifar10 dataset")
       transform=transforms.Compose(
           [transforms.ToTensor(),
            transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])
   
       trainset=torchvision.datasets.CIFAR10(root=args.data_dir, train=True,
                                               download=False, transform=transform)
       train_loader=torch.utils.data.DataLoader(trainset, batch_size=args.batch_size,
                                                  shuffle=True, num_workers=args.workers)
   
       testset=torchvision.datasets.CIFAR10(root=args.data_dir, train=False,
                                              download=False, transform=transform)
       test_loader=torch.utils.data.DataLoader(testset, batch_size=args.batch_size,
                                                 shuffle=False, num_workers=args.workers)
   
       logger.info("Model loaded")
       model=Net()
   
       if torch.cuda.device_count() > 1:
           logger.info("Gpu count: {}".format(torch.cuda.device_count()))
           model=nn.DataParallel(model)
   
       model=model.to(device)
   
       criterion=nn.CrossEntropyLoss().to(device)
       optimizer=torch.optim.SGD(model.parameters(), lr=args.lr, momentum=args.momentum)
   
       for epoch in range(0, args.epochs):
           running_loss=0.0
           for i, data in enumerate(train_loader):
               # get the inputs
               inputs, labels=data
               inputs, labels=inputs.to(device), labels.to(device)
   
               # zero the parameter gradients
               optimizer.zero_grad()
   
               # forward + backward + optimize
               outputs=model(inputs)
               loss=criterion(outputs, labels)
               loss.backward()
               optimizer.step()
   
               # print statistics
               running_loss += loss.item()
               if i % 2000 == 1999:  # print every 2000 mini-batches
                   print('[%d, %5d] loss: %.3f' %
                         (epoch + 1, i + 1, running_loss / 2000))
                   running_loss=0.0
       print('Finished Training')
       return _save_model(model, args.model_dir)
   
   
   def _save_model(model, model_dir):
       logger.info("Saving the model.")
       path=os.path.join(model_dir, 'model.pth')
       # recommended way from http://pytorch.org/docs/master/notes/serialization.html
       torch.save(model.cpu().state_dict(), path)
   
   
   def model_fn(model_dir):
       logger.info('model_fn')
       device="cuda" if torch.cuda.is_available() else "cpu"
       model=Net()
       if torch.cuda.device_count() > 1:
           logger.info("Gpu count: {}".format(torch.cuda.device_count()))
           model=nn.DataParallel(model)
   
       with open(os.path.join(model_dir, 'model.pth'), 'rb') as f:
           model.load_state_dict(torch.load(f))
       return model.to(device)
   
   
   if __name__ == '__main__':
       parser=argparse.ArgumentParser()
   
       parser.add_argument('--workers', type=int, default=2, metavar='W',
                           help='number of data loading workers (default: 2)')
       parser.add_argument('--epochs', type=int, default=2, metavar='E',
                           help='number of total epochs to run (default: 2)')
       parser.add_argument('--batch-size', type=int, default=4, metavar='BS',
                           help='batch size (default: 4)')
       parser.add_argument('--lr', type=float, default=0.001, metavar='LR',
                           help='initial learning rate (default: 0.001)')
       parser.add_argument('--momentum', type=float, default=0.9, metavar='M', help='momentum (default: 0.9)')
       parser.add_argument('--dist-backend', type=str, default='gloo', help='distributed backend (default: gloo)')
   
       # The parameters below retrieve their default values from SageMaker environment variables, which are
       # instantiated by the SageMaker containers framework.
       # https://github.com/aws/sagemaker-containers#how-a-script-is-executed-inside-the-container
       parser.add_argument('--hosts', type=str, default=ast.literal_eval(os.environ['SM_HOSTS']))
       parser.add_argument('--current-host', type=str, default=os.environ['SM_CURRENT_HOST'])
       parser.add_argument('--model-dir', type=str, default=os.environ['SM_MODEL_DIR'])
       parser.add_argument('--data-dir', type=str, default=os.environ['SM_CHANNEL_TRAINING'])
       parser.add_argument('--num-gpus', type=int, default=os.environ['SM_NUM_GPUS'])
   
       _train(parser.parse_args())
   ```

### Step 3: Build the Container<a name="extend-step3"></a>

1. In the JupyterLab home directory, open a Jupyter notebook\. To open a new notebook, choose the **New Launch** icon and then choose **conda\_pytorch\_p39** in the **Notebook** section\. 

1. Run the following command in the first notebook cell to change to the `docker_test_folder` directory:

   ```
   % cd ~/SageMaker/docker_test_folder
   ```

   This returns your current directory as follows:

   ```
   ! pwd
   ```

   `output: /home/ec2-user/SageMaker/docker_test_folder`

1. Log in to Docker to access the base container:

   ```
   ! aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 763104351884.dkr.ecr.us-east-1.amazonaws.com
   ```

1. To build the Docker container, run the following Docker build command, including the space followed by a period at the end:

   ```
   ! docker build -t pytorch-extended-container-test .
   ```

   The Docker build command must be run from the Docker directory you created, in this case `docker_test_folder`\.
**Note**  
If you get the following error message that Docker cannot find the Dockerfile, make sure the Dockerfile has the correct name and has been saved to the directory\.  

   ```
   unable to prepare context: unable to evaluate symlinks in Dockerfile path: 
   lstat /home/ec2-user/SageMaker/docker/Dockerfile: no such file or directory
   ```
Remember that `docker` looks for a file specifically called `Dockerfile` without any extension within the current directory\. If you named it something else, you can pass in the file name manually with the `-f` flag\. For example, if you named your Dockerfile `Dockerfile-text.txt`, run the following command:  

   ```
   ! docker build -t tf-custom-container-test -f Dockerfile-text.txt .
   ```

### Step 4: Test the Container<a name="extend-step4"></a>

1. To test the container locally in the notebook instance, open a Jupyter notebook\. Choose **New Launcher** and choose **Notebook** in **`conda_pytorch_p39`** framework\. The rest of the code snippets must run from the Jupyter notebook instance\.

1. Download the CIFAR\-10 dataset\.

   ```
   import torch
   import torchvision
   import torchvision.transforms as transforms
   
   def _get_transform():
       return transforms.Compose(
           [transforms.ToTensor(),
            transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])
   
   
   def get_train_data_loader(data_dir='/tmp/pytorch/cifar-10-data'):
       transform=_get_transform()
       trainset=torchvision.datasets.CIFAR10(root=data_dir, train=True,
                                               download=True, transform=transform)
       return torch.utils.data.DataLoader(trainset, batch_size=4,
                                          shuffle=True, num_workers=2)
   
   
   def get_test_data_loader(data_dir='/tmp/pytorch/cifar-10-data'):
       transform=_get_transform()
       testset=torchvision.datasets.CIFAR10(root=data_dir, train=False,
                                              download=True, transform=transform)
       return torch.utils.data.DataLoader(testset, batch_size=4,
                                          shuffle=False, num_workers=2)
   
   trainloader=get_train_data_loader('/tmp/pytorch-example/cifar-10-data')
   testloader=get_test_data_loader('/tmp/pytorch-example/cifar-10-data')
   ```

1. Set `role` to the role used to create your Jupyter notebook\. This is used to configure your SageMaker Estimator\.

   ```
   from sagemaker import get_execution_role
   
   role=get_execution_role()
   ```

1. Paste the following example script into the notebook code cell to configure a SageMaker Estimator using your extended container\.

   ```
   from sagemaker.estimator import Estimator
   
   hyperparameters={'epochs': 1}
   
   estimator=Estimator(
       image_uri='pytorch-extended-container-test',
       role=role,
       instance_count=1,
       instance_type='local',
       hyperparameters=hyperparameters
   )
   
   estimator.fit('file:///tmp/pytorch-example/cifar-10-data')
   ```

1. Run the code cell\. This test outputs the training environment configuration, the values used for the environmental variables, the source of the data, and the loss and accuracy obtained during training\.

### Step 5: Push the Container to Amazon Elastic Container Registry \(Amazon ECR\)<a name="extend-step5"></a>

1. After you successfully run the local mode test, you can push the Docker container to [Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) and use it to run training jobs\. 

   Run the following command lines in a notebook cell\.

   ```
   %%sh
   
   # Specify an algorithm name
   algorithm_name=pytorch-extended-container-test
   
   account=$(aws sts get-caller-identity --query Account --output text)
   
   # Get the region defined in the current configuration (default to us-west-2 if none defined)
   region=$(aws configure get region)
   
   fullname="${account}.dkr.ecr.${region}.amazonaws.com/${algorithm_name}:latest"
   
   # If the repository doesn't exist in ECR, create it.
   
   aws ecr describe-repositories --repository-names "${algorithm_name}" > /dev/null 2>&1
   if [ $? -ne 0 ]
   then
   aws ecr create-repository --repository-name "${algorithm_name}" > /dev/null
   fi
   
   # Log into Docker
   aws ecr get-login-password --region ${region}|docker login --username AWS --password-stdin ${fullname}
   
   # Build the docker image locally with the image name and then push it to ECR
   # with the full name.
   
   docker build -t ${algorithm_name} .
   docker tag ${algorithm_name} ${fullname}
   
   docker push ${fullname}
   ```

1. After you push the container, you can call the Amazon ECR image from anywhere in the SageMaker environment\. Run the following code example in the next notebook cell\. 

   If you want to use this training container with SageMaker Studio to use its visualization features, you can also run the following code in a Studio notebook cell to call the Amazon ECR image of your training container\.

   ```
   import boto3
   
   client=boto3.client('sts')
   account=client.get_caller_identity()['Account']
   
   my_session=boto3.session.Session()
   region=my_session.region_name
   
   algorithm_name="pytorch-extended-container-test"
   ecr_image='{}.dkr.ecr.{}.amazonaws.com/{}:latest'.format(account, region, algorithm_name)
   
   ecr_image
   # This should return something like
   # 12-digits-of-your-account.dkr.ecr.us-east-2.amazonaws.com/tf-2.2-test:latest
   ```

1. Use the `ecr_image` retrieved from the previous step to configure a SageMaker estimator object\. The following code sample configures a SageMaker PyTorch estimator\.

   ```
   import sagemaker
   
   from sagemaker import get_execution_role
   from sagemaker.estimator import Estimator
   
   estimator=Estimator(
       image_uri=ecr_image,
       role=get_execution_role(),
       base_job_name='pytorch-extended-container-test',
       instance_count=1,
       instance_type='ml.p2.xlarge'
   )
   
   # start training
   estimator.fit()
   
   # deploy the trained model
   predictor=estimator.deploy(1, instance_type)
   ```

### Step 6: Clean up Resources<a name="extend-step6"></a>

**To clean up resources when done with the Get Started example**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/), choose the notebook instance **RunScriptNotebookInstance**, choose **Actions**, and choose **Stop**\. It can take a few minutes for the instance to stop\. 

1. After the instance **Status** changes to **Stopped**, choose **Actions**, choose **Delete**, and then choose **Delete** in the dialog box\. It can take a few minutes for the instance to be deleted\. The notebook instance disappears from the table when it has been deleted\. 

1. Open the [Amazon S3 console](https://console.aws.amazon.com/s3/) and delete the bucket that you created for storing model artifacts and the training dataset\. 

1. Open the [IAM console](https://console.aws.amazon.com/iam/) and delete the IAM role\. If you created permission policies, you can delete them, too\. 
**Note**  
 The Docker container shuts down automatically after it has run\. You don't need to delete it\.