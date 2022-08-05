# Step 2: Launch a SageMaker Distributed Training Job Using the SageMaker Python SDK<a name="data-parallel-use-api"></a>

To run your adapted script in [Step 1: Modify Your Own Training Script](data-parallel-modify-sdp-select-framework.md), start with creating a SageMaker framework or generic estimator object with the prepared training script and the distributed training configuration parameter\. You can use the library in any kind of SageMaker environment and web IDEs, such as [SageMaker notebook instance](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) and [SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html)\.

Use the high\-level [SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/api/training/index.html) to do one of the following:
+ If you want to achieve a quick adoption of your distributed training job in SageMaker, configure a SageMaker [PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html#sagemaker.pytorch.estimator.PyTorch) or [TensorFlow](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) framework estimator class\. The framework estimator picks up your training script and automatically matches the right image URI of the [pre\-built PyTorch or TensorFlow Deep Learning Containers \(DLC\)](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-framework-containers-sm-support-only), given the value specified to the `framework_version` parameter\.
+ If you want to extend one of the pre\-built containers or build a custom container to create your own ML environment with SageMaker, use the SageMaker generic `Estimator` class and specify the image URI of the custom Docker container hosted in your Amazon Elastic Container Registry \(Amazon ECR\)\.

Your training datasets should be stored in Amazon S3 or [Amazon FSx for Lustre](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html) in the AWS Region in which you are launching your training job\. If you use Jupyter notebooks, you should have a SageMaker notebook instance or a SageMaker Studio app running in the same AWS Region\. For more information about storing your training data, see the [SageMaker Python SDK data inputs](https://sagemaker.readthedocs.io/en/stable/overview.html#use-file-systems-as-training-input) documentation\. 

**Tip**  
We highly recommend that you use Amazon FSx for Lustre instead of Amazon S3 to increase training performance\. Amazon FSx has higher throughput and lower latency than Amazon S3\. 

Choose one of the following topics for instructions on how to run your TensorFlow or PyTorch training scripts\. After you launch a training job, you can monitor system utilization and model performance using [Amazon SageMaker Debugger](train-debugger.md) or Amazon CloudWatch\.

While you follow instructions in the following topics to learn more about technical details, we also recommend that you try the [Amazon SageMaker Distributed Training Notebook Examples](distributed-training-notebook-examples.md) to get started\.

**Topics**
+ [Using the SageMaker Framework Estimators For PyTorch and TensorFlow](#data-parallel-framework-estimator)
+ [Using the SageMaker Generic Estimator to Extend Prebuilt Containers](#data-parallel-use-python-skd-api)
+ [Create Your Own Docker Container with the SageMaker Distributed Data Parallel Library](#data-parallel-bring-your-own-container)

## Using the SageMaker Framework Estimators For PyTorch and TensorFlow<a name="data-parallel-framework-estimator"></a>

You can activate the SageMaker [distributed data parallel library](https://sagemaker.readthedocs.io/en/stable/api/training/smd_data_parallel.html) by specifying it as the `distribution` strategy in the [SageMaker framework estimator class](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator)\.

------
#### [ Using the SageMaker PyTorch estimator ]

```
from sagemaker.pytorch import PyTorch

pt_estimator = PyTorch(
    base_job_name="training_job_name_prefix",
    entry_point="adapted-training-script.py",
    role="SageMakerRole",
    py_version="py38",
    framework_version="1.12.0",

    # For running a multi-node distributed training job, specify a value greater than 1
    # Example: 2,3,4,..8
    instance_count=2,

    # Instance types supported by the SageMaker data parallel library: 
    # ml.p4d.24xlarge, ml.p3dn.24xlarge, and ml.p3.16xlarge
    instance_type="ml.p3.16xlarge",

    # Training using the SageMaker data parallel distributed training strategy
    distribution={ "smdistributed": { "dataparallel": { "enabled": True } } }
)

pt_estimator.fit("s3://bucket/path/to/training/data")
```

------
#### [ Using the SageMaker TensorFlow estimator ]

```
from sagemaker.tensorflow import TensorFlow

tf_estimator = TensorFlow(
    base_job_name = "training_job_name_prefix",
    entry_point="adapted-training-script.py",
    role="SageMakerRole",
    framework_version="2.9.1",
    py_version="py38",

    # For running a multi-node distributed training job, specify a value greater than 1
    # Example: 2,3,4,..8
    instance_count=2,

    # Instance types supported by the SageMaker data parallel library: 
    # ml.p4d.24xlarge, ml.p3dn.24xlarge, and ml.p3.16xlarge
    instance_type="ml.p3.16xlarge",

    # Training using the SageMaker data parallel distributed training strategy
    distribution={ "smdistributed": { "dataparallel": { "enabled": True } } }
)

tf_estimator.fit("s3://bucket/path/to/training/data")
```

------

The following two parameters of the SageMaker framework estimator are required to activate the SageMaker data parallelism\. 

**`distribution`** \(dict\): A dictionary with information on how to run distributed training \(default: `None`\)\.  
+ To use `smdistributed.dataparallel` as a distribution strategy, configure a dictionary as shown in the following code: 

  ```
  distribution = { "smdistributed": { "dataparallel": { "enabled": True } } } 
  ```
+ **`custom_mpi_options`** \(str\)\(optional\): Custom MPI options\. The following is an example of how you can use this parameter when defining `distribution`\. To learn more, see [Custom MPI Options](data-parallel-config.md#data-parallel-config-mpi-custom)\.

  ```
  distribution = { 
      "smdistributed": { 
          "dataparallel": {
              "enabled": True, 
              "custom_mpi_options": "-verbose -x NCCL_DEBUG=VERSION"
          }
      }
  }
  ```

 **`instance_type`** \(str\): A type of Amazon EC2 instance to use\. 
+ If using the `smdistributed` with `dataparallel` distribution strategy, you must use one of the following instance types: `ml.p4d.24xlarge`, `ml.p3dn.24xlarge`, and `ml.p3.16xlarge`\. For best performance, we recommend that you use an EFA\-enabled instance, which are `ml.p3dn.24xlarge` and `ml.p4d.24xlarge`\.

## Using the SageMaker Generic Estimator to Extend Prebuilt Containers<a name="data-parallel-use-python-skd-api"></a>

You can customize SageMaker prebuilt containers or extend them to handle any additional functional requirements for your algorithm or model that the prebuilt SageMaker Docker image doesn't support\. For an example of how you can extend a pre\-built container, see [Extend a Prebuilt Container](https://docs.aws.amazon.com/sagemaker/latest/dg/prebuilt-containers-extend.html)\.

To extend a prebuilt container or adapt your own container to use the library, you must use one of the images listed in [Supported Frameworks](distributed-data-parallel-support.md#distributed-data-parallel-supported-frameworks)\.

**Important**  
From TensorFlow 2\.4\.1 and PyTorch 1\.8\.1, the framework DLCs supports EFA\-enabled instance types \(`ml.p3dn.24xlarge`, `ml.p4d.24xlarge`\)\. We recommend that you use the DLC images that contain TensorFlow 2\.4\.1 or later and PyTorch 1\.8\.1 or later\. 

For example, if you use PyTorch, your Dockerfile should contain a `FROM` statement similar to the following:

```
# SageMaker PyTorch image
FROM 763104351884.dkr.ecr.<aws-region>.amazonaws.com/pytorch-training:<image-tag>

ENV PATH="/opt/ml/code:${PATH}"

# this environment variable is used by the SageMaker PyTorch container to determine our user code directory.
ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code

# /opt/ml and all subdirectories are utilized by SageMaker, use the /code subdirectory to store your user code.
COPY cifar10.py /opt/ml/code/cifar10.py

# Defines cifar10.py as script entrypoint
ENV SAGEMAKER_PROGRAM cifar10.py
```

You can further customize your own Docker container to work with SageMaker using the [SageMaker Training toolkit](https://github.com/aws/sagemaker-training-toolkit) and the binary file of the SageMaker distributed data parallel library\. To learn more, see the instructions in the following section\.

## Create Your Own Docker Container with the SageMaker Distributed Data Parallel Library<a name="data-parallel-bring-your-own-container"></a>

To build your own Docker container for training and use the SageMaker data parallel library, you must include the correct dependencies and the binary files of the SageMaker distributed parallel libraries in your Dockerfile\. This section provides instructions on how to create a complete Dockerfile with the minimum set of dependencies for distributed training in SageMaker using the data parallel library\.

**Note**  
This custom Docker option with the SageMaker data parallel library as a binary is available only for PyTorch\.

**To create a Dockerfile with the SageMaker training toolkit and the data parallel library**

1. Start with a Docker image from [NVIDIA CUDA](https://hub.docker.com/r/nvidia/cuda)\. Use the cuDNN developer versions that contain CUDA runtime and development tools \(headers and libraries\) to build from the [PyTorch source code](https://github.com/pytorch/pytorch#from-source)\.

   ```
   FROM nvidia/cuda:11.3.1-cudnn8-devel-ubuntu20.04
   ```
**Tip**  
The official AWS Deep Learning Container \(DLC\) images are built from the [NVIDIA CUDA base images](https://hub.docker.com/r/nvidia/cuda)\. If you want to use the prebuilt DLC images as references while following the rest of the instructions, see the [AWS Deep Learning Containers for PyTorch Dockerfiles](https://github.com/aws/deep-learning-containers/tree/master/pytorch)\. 

1. Add the following arguments to specify versions of PyTorch and other packages\. Also, indicate the Amazon S3 bucket paths to the SageMaker data parallel library and other software to use AWS resources, such as the Amazon S3 plug\-in\. 

   To use versions of the third party libraries other than the ones provided in the following code example, we recommend you look into the [official Dockerfiles of AWS Deep Learning Container for PyTorch](https://github.com/aws/deep-learning-containers/tree/master/pytorch/training/docker) to find versions that are tested, compatible, and suitable for your application\. 

   To find URLs for the `SMDATAPARALLEL_BINARY` argument, see the look up tables at [Supported Frameworks](distributed-data-parallel-support.md#distributed-data-parallel-supported-frameworks)\.

   ```
   ARG PYTORCH_VERSION=1.10.2
   ARG PYTHON_SHORT_VERSION=3.8
   ARG EFA_VERSION=1.14.1
   ARG SMDATAPARALLEL_BINARY=https://smdataparallel.s3.amazonaws.com/binary/pytorch/${PYTORCH_VERSION}/cu113/2022-02-18/smdistributed_dataparallel-1.4.0-cp38-cp38-linux_x86_64.whl
   ARG PT_S3_WHL_GPU=https://aws-s3-plugin.s3.us-west-2.amazonaws.com/binaries/0.0.1/1c3e69e/awsio-0.0.1-cp38-cp38-manylinux1_x86_64.whl
   ARG CONDA_PREFIX="/opt/conda"
   ARG BRANCH_OFI=1.1.3-aws
   ```

1. Set the following environment variables to properly build SageMaker training components and run the data parallel library\. You use these variables for the components in the subsequent steps\.

   ```
   # Set ENV variables required to build PyTorch
   ENV TORCH_CUDA_ARCH_LIST="7.0+PTX 8.0"
   ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
   ENV NCCL_VERSION=2.10.3
   
   # Add OpenMPI to the path.
   ENV PATH /opt/amazon/openmpi/bin:$PATH
   
   # Add Conda to path
   ENV PATH $CONDA_PREFIX/bin:$PATH
   
   # Set this enviroment variable for SageMaker to launch SMDDP correctly.
   ENV SAGEMAKER_TRAINING_MODULE=sagemaker_pytorch_container.training:main
   
   # Add enviroment variable for processes to be able to call fork()
   ENV RDMAV_FORK_SAFE=1
   
   # Indicate the container type
   ENV DLC_CONTAINER_TYPE=training
   
   # Add EFA and SMDDP to LD library path
   ENV LD_LIBRARY_PATH="/opt/conda/lib/python${PYTHON_SHORT_VERSION}/site-packages/smdistributed/dataparallel/lib:$LD_LIBRARY_PATH"
   ENV LD_LIBRARY_PATH=/opt/amazon/efa/lib/:$LD_LIBRARY_PATH
   ```

1. Install or update `curl`, `wget`, and `git` to download and build packages in the subsequent steps\.

   ```
   RUN --mount=type=cache,id=apt-final,target=/var/cache/apt \
       apt-get update && apt-get install -y  --no-install-recommends \
           curl \
           wget \
           git \
       && rm -rf /var/lib/apt/lists/*
   ```

1. Install [Elastic Fabric Adapter \(EFA\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/efa.html) software for Amazon EC2 network communication\.

   ```
   RUN DEBIAN_FRONTEND=noninteractive apt-get update
   RUN mkdir /tmp/efa \
       && cd /tmp/efa \
       && curl --silent -O https://efa-installer.amazonaws.com/aws-efa-installer-${EFA_VERSION}.tar.gz \
       && tar -xf aws-efa-installer-${EFA_VERSION}.tar.gz \
       && cd aws-efa-installer \
       && ./efa_installer.sh -y --skip-kmod -g \
       && rm -rf /tmp/efa
   ```

1. Install [Conda](https://docs.conda.io/en/latest/) to handle package management\. 

   ```
   RUN curl -fsSL -v -o ~/miniconda.sh -O  https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh  && \
       chmod +x ~/miniconda.sh && \
       ~/miniconda.sh -b -p $CONDA_PREFIX && \
       rm ~/miniconda.sh && \
       $CONDA_PREFIX/bin/conda install -y python=${PYTHON_SHORT_VERSION} conda-build pyyaml numpy ipython && \
       $CONDA_PREFIX/bin/conda clean -ya
   ```

1. Get, build, and install PyTorch and its dependencies\. We build [PyTorch from the source code](https://github.com/pytorch/pytorch#from-source) because we need to have control of the NCCL version to guarantee compatibility with the [AWS OFI NCCL plug\-in](https://github.com/aws/aws-ofi-nccl)\.

   1. Following the steps in the [PyTorch official dockerfile](https://github.com/pytorch/pytorch/blob/master/Dockerfile), install build dependencies and set up [ccache](https://ccache.dev/) to speed up recompilation\.

      ```
      RUN DEBIAN_FRONTEND=noninteractive \
          apt-get install -y --no-install-recommends \
              build-essential \
              ca-certificates \
              ccache \
              cmake \
              git \
              libjpeg-dev \
              libpng-dev \
          && rm -rf /var/lib/apt/lists/*
        
      # Setup ccache
      RUN /usr/sbin/update-ccache-symlinks
      RUN mkdir /opt/ccache && ccache --set-config=cache_dir=/opt/ccache
      ```

   1. Install [PyTorch’s common and Linux dependencies](https://github.com/pytorch/pytorch#install-dependencies)\.

      ```
      # Common dependencies for PyTorch
      RUN conda install astunparse numpy ninja pyyaml mkl mkl-include setuptools cmake cffi typing_extensions future six requests dataclasses
      
      # Linux specific dependency for PyTorch
      RUN conda install -c pytorch magma-cuda113
      ```

   1. Clone the [PyTorch GitHub repository](https://github.com/pytorch/pytorch)\.

      ```
      RUN --mount=type=cache,target=/opt/ccache \
          cd / \
          && git clone --recursive https://github.com/pytorch/pytorch -b v${PYTORCH_VERSION}
      ```

   1. Install and build a specific [NCCL](https://developer.nvidia.com/nccl) version\. To do this, replace the content in the PyTorch’s default NCCL folder \(`/pytorch/third_party/nccl`\) with the specific NCCL version from the NVIDIA repository\. The NCCL version was set in the step 3 of this guide\.

      ```
      RUN cd /pytorch/third_party/nccl \
          && rm -rf nccl \
          && git clone https://github.com/NVIDIA/nccl.git -b v${NCCL_VERSION}-1 \
          && cd nccl \
          && make -j64 src.build CUDA_HOME=/usr/local/cuda NVCC_GENCODE="-gencode=arch=compute_70,code=sm_70 -gencode=arch=compute_80,code=sm_80" \
          && make pkg.txz.build \
          && tar -xvf build/pkg/txz/nccl_*.txz -C $CONDA_PREFIX --strip-components=1
      ```

   1. Build and install PyTorch\. This process usually takes slightly more than one hour to complete\. It is built using the NCCL version downloaded in a previous step\.

      ```
      RUN cd /pytorch \
          && CMAKE_PREFIX_PATH="$(dirname $(which conda))/../" \
          python setup.py install \
          && rm -rf /pytorch
      ```

1. Build and install [AWS OFI NCCL plugin](https://github.com/aws/aws-ofi-nccl)\. This enables [libfabric](https://github.com/ofiwg/libfabric) support for the SageMaker data parallel library\.

   ```
   RUN DEBIAN_FRONTEND=noninteractive apt-get update \
       && apt-get install -y --no-install-recommends \
           autoconf \
           automake \
           libtool
   RUN mkdir /tmp/efa-ofi-nccl \
       && cd /tmp/efa-ofi-nccl \
       && git clone https://github.com/aws/aws-ofi-nccl.git -b v${BRANCH_OFI} \
       && cd aws-ofi-nccl \
       && ./autogen.sh \
       && ./configure --with-libfabric=/opt/amazon/efa \
       --with-mpi=/opt/amazon/openmpi \
       --with-cuda=/usr/local/cuda \
       --with-nccl=$CONDA_PREFIX \
       && make \
       && make install \
       && rm -rf /tmp/efa-ofi-nccl
   ```

1. Build and install [TorchVision](https://github.com/pytorch/vision.git)\.

   ```
   RUN pip install --no-cache-dir -U \
       packaging \
       mpi4py==3.0.3
   RUN cd /tmp \
       && git clone https://github.com/pytorch/vision.git -b v0.9.1 \
       && cd vision \
       && BUILD_VERSION="0.9.1+cu111" python setup.py install \
       && cd /tmp \
       && rm -rf vision
   ```

1. Install and configure OpenSSH\. OpenSSH is required for MPI to communicate between containers\. Allow OpenSSH to talk to containers without asking for confirmation\.

   ```
   RUN apt-get update \
       && apt-get install -y  --allow-downgrades --allow-change-held-packages --no-install-recommends \
       && apt-get install -y --no-install-recommends openssh-client openssh-server \
       && mkdir -p /var/run/sshd \
       && cat /etc/ssh/ssh_config | grep -v StrictHostKeyChecking > /etc/ssh/ssh_config.new \
       && echo "    StrictHostKeyChecking no" >> /etc/ssh/ssh_config.new \
       && mv /etc/ssh/ssh_config.new /etc/ssh/ssh_config \
       && rm -rf /var/lib/apt/lists/*
   
   # Configure OpenSSH so that nodes can communicate with each other
   RUN mkdir -p /var/run/sshd && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
   RUN rm -rf /root/.ssh/ && \
    mkdir -p /root/.ssh/ && \
    ssh-keygen -q -t rsa -N '' -f /root/.ssh/id_rsa && \
    cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys \
    && printf "Host *\n StrictHostKeyChecking no\n" >> /root/.ssh/config
   ```

1. Install the PT S3 plug\-in to efficiently access datasets in Amazon S3\.

   ```
   RUN pip install --no-cache-dir -U ${PT_S3_WHL_GPU}
   RUN mkdir -p /etc/pki/tls/certs && cp /etc/ssl/certs/ca-certificates.crt /etc/pki/tls/certs/ca-bundle.crt
   ```

1. Install the [libboost](https://www.boost.org/) library\. This package is needed for networking the asynchronous IO functionality of the SageMaker data parallel library\.

   ```
   WORKDIR /
   RUN wget https://sourceforge.net/projects/boost/files/boost/1.73.0/boost_1_73_0.tar.gz/download -O boost_1_73_0.tar.gz \
       && tar -xzf boost_1_73_0.tar.gz \
       && cd boost_1_73_0 \
       && ./bootstrap.sh \
       && ./b2 threading=multi --prefix=${CONDA_PREFIX} -j 64 cxxflags=-fPIC cflags=-fPIC install || true \
       && cd .. \
       && rm -rf boost_1_73_0.tar.gz \
       && rm -rf boost_1_73_0 \
       && cd ${CONDA_PREFIX}/include/boost
   ```

1. Install the following SageMaker tools for PyTorch training\.

   ```
   WORKDIR /root
   RUN pip install --no-cache-dir -U \
       smclarify \
       "sagemaker>=2,<3" \
       sagemaker-experiments==0.* \
       sagemaker-pytorch-training
   ```

1. Finally, install the SageMaker data parallel binary and the remaining dependencies\.

   ```
   RUN --mount=type=cache,id=apt-final,target=/var/cache/apt \
     apt-get update && apt-get install -y  --no-install-recommends \
     jq \
     libhwloc-dev \
     libnuma1 \
     libnuma-dev \
     libssl1.1 \
     libtool \
     hwloc \
     && rm -rf /var/lib/apt/lists/*
   
   RUN SMDATAPARALLEL_PT=1 pip install --no-cache-dir ${SMDATAPARALLEL_BINARY}
   ```

1. After you finish creating the Dockerfile, see [Adapting Your Own Training Container](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html) to learn how to build the Docker container, host it in Amazon ECR, and run a training job using the SageMaker Python SDK\.

The following example code shows a complete Dockerfile after combining all the previous code blocks\.

```
# This file creates a docker image with minimum dependencies to run SageMaker data parallel training
FROM nvidia/cuda:11.3.1-cudnn8-devel-ubuntu20.04

# Set appropiate versions and location for components
ARG PYTORCH_VERSION=1.10.2
ARG PYTHON_SHORT_VERSION=3.8
ARG EFA_VERSION=1.14.1
ARG SMDATAPARALLEL_BINARY=https://smdataparallel.s3.amazonaws.com/binary/pytorch/${PYTORCH_VERSION}/cu113/2022-02-18/smdistributed_dataparallel-1.4.0-cp38-cp38-linux_x86_64.whl
ARG PT_S3_WHL_GPU=https://aws-s3-plugin.s3.us-west-2.amazonaws.com/binaries/0.0.1/1c3e69e/awsio-0.0.1-cp38-cp38-manylinux1_x86_64.whl
ARG CONDA_PREFIX="/opt/conda"
ARG BRANCH_OFI=1.1.3-aws

# Set ENV variables required to build PyTorch
ENV TORCH_CUDA_ARCH_LIST="3.7 5.0 7.0+PTX 8.0"
ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
ENV NCCL_VERSION=2.10.3

# Add OpenMPI to the path.
ENV PATH /opt/amazon/openmpi/bin:$PATH

# Add Conda to path
ENV PATH $CONDA_PREFIX/bin:$PATH

# Set this enviroment variable for SageMaker to launch SMDDP correctly.
ENV SAGEMAKER_TRAINING_MODULE=sagemaker_pytorch_container.training:main

# Add enviroment variable for processes to be able to call fork()
ENV RDMAV_FORK_SAFE=1

# Indicate the container type
ENV DLC_CONTAINER_TYPE=training

# Add EFA and SMDDP to LD library path
ENV LD_LIBRARY_PATH="/opt/conda/lib/python${PYTHON_SHORT_VERSION}/site-packages/smdistributed/dataparallel/lib:$LD_LIBRARY_PATH"
ENV LD_LIBRARY_PATH=/opt/amazon/efa/lib/:$LD_LIBRARY_PATH

# Install basic dependencies to download and build other dependencies
RUN --mount=type=cache,id=apt-final,target=/var/cache/apt \
  apt-get update && apt-get install -y  --no-install-recommends \
  curl \
  wget \
  git \
  && rm -rf /var/lib/apt/lists/*

# Install EFA.
# This is required for SMDDP backend communication
RUN DEBIAN_FRONTEND=noninteractive apt-get update
RUN mkdir /tmp/efa \
    && cd /tmp/efa \
    && curl --silent -O https://efa-installer.amazonaws.com/aws-efa-installer-${EFA_VERSION}.tar.gz \
    && tar -xf aws-efa-installer-${EFA_VERSION}.tar.gz \
    && cd aws-efa-installer \
    && ./efa_installer.sh -y --skip-kmod -g \
    && rm -rf /tmp/efa

# Install Conda
RUN curl -fsSL -v -o ~/miniconda.sh -O  https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh  && \
    chmod +x ~/miniconda.sh && \
    ~/miniconda.sh -b -p $CONDA_PREFIX && \
    rm ~/miniconda.sh && \
    $CONDA_PREFIX/bin/conda install -y python=${PYTHON_SHORT_VERSION} conda-build pyyaml numpy ipython && \
    $CONDA_PREFIX/bin/conda clean -ya

# Install PyTorch.
# Start with dependencies listed in official PyTorch dockerfile
# https://github.com/pytorch/pytorch/blob/master/Dockerfile
RUN DEBIAN_FRONTEND=noninteractive \
    apt-get install -y --no-install-recommends \
        build-essential \
        ca-certificates \
        ccache \
        cmake \
        git \
        libjpeg-dev \
        libpng-dev && \
    rm -rf /var/lib/apt/lists/*

# Setup ccache
RUN /usr/sbin/update-ccache-symlinks
RUN mkdir /opt/ccache && ccache --set-config=cache_dir=/opt/ccache

# Common dependencies for PyTorch
RUN conda install astunparse numpy ninja pyyaml mkl mkl-include setuptools cmake cffi typing_extensions future six requests dataclasses

# Linux specific dependency for PyTorch
RUN conda install -c pytorch magma-cuda113

# Clone PyTorch
RUN --mount=type=cache,target=/opt/ccache \
    cd / \
    && git clone --recursive https://github.com/pytorch/pytorch -b v${PYTORCH_VERSION}
# Note that we need to use the same NCCL version for PyTorch and OFI plugin.
# To enforce that, install NCCL from source before building PT and OFI plugin.

# Install NCCL.
# Required for building OFI plugin (OFI requires NCCL's header files and library)
RUN cd /pytorch/third_party/nccl \
    && rm -rf nccl \
    && git clone https://github.com/NVIDIA/nccl.git -b v${NCCL_VERSION}-1 \
    && cd nccl \
    && make -j64 src.build CUDA_HOME=/usr/local/cuda NVCC_GENCODE="-gencode=arch=compute_70,code=sm_70 -gencode=arch=compute_80,code=sm_80" \
    && make pkg.txz.build \
    && tar -xvf build/pkg/txz/nccl_*.txz -C $CONDA_PREFIX --strip-components=1

# Build and install PyTorch.
RUN cd /pytorch \
    && CMAKE_PREFIX_PATH="$(dirname $(which conda))/../" \
    python setup.py install \
    && rm -rf /pytorch

RUN ccache -C

# Build and install OFI plugin. \
# It is required to use libfabric.
RUN DEBIAN_FRONTEND=noninteractive apt-get update \
    && apt-get install -y --no-install-recommends \
        autoconf \
        automake \
        libtool
RUN mkdir /tmp/efa-ofi-nccl \
    && cd /tmp/efa-ofi-nccl \
    && git clone https://github.com/aws/aws-ofi-nccl.git -b v${BRANCH_OFI} \
    && cd aws-ofi-nccl \
    && ./autogen.sh \
    && ./configure --with-libfabric=/opt/amazon/efa \
        --with-mpi=/opt/amazon/openmpi \
        --with-cuda=/usr/local/cuda \
        --with-nccl=$CONDA_PREFIX \
    && make \
    && make install \
    && rm -rf /tmp/efa-ofi-nccl

# Build and install Torchvision
RUN pip install --no-cache-dir -U \
    packaging \
    mpi4py==3.0.3
RUN cd /tmp \
    && git clone https://github.com/pytorch/vision.git -b v0.9.1 \
    && cd vision \
    && BUILD_VERSION="0.9.1+cu111" python setup.py install \
    && cd /tmp \
    && rm -rf vision

# Install OpenSSH.
# Required for MPI to communicate between containers, allow OpenSSH to talk to containers without asking for confirmation
RUN apt-get update \
    && apt-get install -y  --allow-downgrades --allow-change-held-packages --no-install-recommends \
    && apt-get install -y --no-install-recommends openssh-client openssh-server \
    && mkdir -p /var/run/sshd \
    && cat /etc/ssh/ssh_config | grep -v StrictHostKeyChecking > /etc/ssh/ssh_config.new \
    && echo "    StrictHostKeyChecking no" >> /etc/ssh/ssh_config.new \
    && mv /etc/ssh/ssh_config.new /etc/ssh/ssh_config \
    && rm -rf /var/lib/apt/lists/*
# Configure OpenSSH so that nodes can communicate with each other
RUN mkdir -p /var/run/sshd && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN rm -rf /root/.ssh/ && \
    mkdir -p /root/.ssh/ && \
    ssh-keygen -q -t rsa -N '' -f /root/.ssh/id_rsa && \
    cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys \
    && printf "Host *\n StrictHostKeyChecking no\n" >> /root/.ssh/config

# Install PT S3 plugin.
# Required to efficiently access datasets in Amazon S3
RUN pip install --no-cache-dir -U ${PT_S3_WHL_GPU}
RUN mkdir -p /etc/pki/tls/certs && cp /etc/ssl/certs/ca-certificates.crt /etc/pki/tls/certs/ca-bundle.crt

# Install libboost from source.
# This package is needed for smdataparallel functionality (for networking asynchronous IO).
WORKDIR /
RUN wget https://sourceforge.net/projects/boost/files/boost/1.73.0/boost_1_73_0.tar.gz/download -O boost_1_73_0.tar.gz \
    && tar -xzf boost_1_73_0.tar.gz \
    && cd boost_1_73_0 \
    && ./bootstrap.sh \
    && ./b2 threading=multi --prefix=${CONDA_PREFIX} -j 64 cxxflags=-fPIC cflags=-fPIC install || true \
    && cd .. \
    && rm -rf boost_1_73_0.tar.gz \
    && rm -rf boost_1_73_0 \
    && cd ${CONDA_PREFIX}/include/boost

# Install SageMaker PyTorch training.
WORKDIR /root
RUN pip install --no-cache-dir -U \
    smclarify \
    "sagemaker>=2,<3" \
    sagemaker-experiments==0.* \
    sagemaker-pytorch-training

# Install SageMaker data parallel binary (SMDDP)
# Start with dependencies
RUN --mount=type=cache,id=apt-final,target=/var/cache/apt \
    apt-get update && apt-get install -y  --no-install-recommends \
        jq \
        libhwloc-dev \
        libnuma1 \
        libnuma-dev \
        libssl1.1 \
        libtool \
        hwloc \
    && rm -rf /var/lib/apt/lists/*

# Install SMDDP
RUN SMDATAPARALLEL_PT=1 pip install --no-cache-dir ${SMDATAPARALLEL_BINARY}
```

**Tip**  
For more general information about creating a custom Dockerfile for training in SageMaker, see [Use Your Own Training Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo.html)\.

**Tip**  
If you want to extend the custom Dockerfile to incorporate the SageMaker model parallel library, see [Create Your Own Docker Container with the SageMaker Distributed Model Parallel Library](model-parallel-sm-sdk.md#model-parallel-bring-your-own-container)\.