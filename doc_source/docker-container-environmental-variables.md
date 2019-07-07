# Reference: Amazon SageMaker Containers Environmental Variables<a name="docker-container-environmental-variables"></a>

The following build\-time environment `` variables are also defined by default when you use the [Amazon SageMaker Containers](https://github.com/aws/sagemaker-containers)\.
+ `SM_NUM_CPUS`

  ```
  SM_NUM_CPUS=32
  ```

  The `SM_NUM_CPUS` environment variable contains the number of CPUs available in the current container\. 

  Example:

  ```
  # Using it in argparse
  parser.add_argument('num_cpus', type=int, default=os.environ['SM_NUM_CPUS'])
  
  # Using it as a variable
  num_cpus = int(os.environ['SM_NUM_CPUS'])
  ```
+ `SM_LOG_LEVEL`

  ```
  SM_LOG_LEVEL=20
  ```

  The `SM_LOG_LEVEL` environment variable contains the current log level in the container\. 

  Example:

  ```
  import logging
  
  logger = logging.getLogger(__name__)
  
  logger.setLevel(int(os.environ.get('SM_LOG_LEVEL', logging.INFO)))
  ```
+ `SM_NETWORK_INTERFACE_NAME`

  ```
  SM_NETWORK_INTERFACE_NAME=ethwe
  ```

  The `SM_NETWORK_INTERFACE_NAME` environment variable contains the name of the network interface, which is used for distributed training\. 

  Example:

  ```
  # Using it in argparse
  parser.add_argument('network_interface', type=str, default=os.environ['SM_NETWORK_INTERFACE_NAME'])
  
  # Using it as a variable
  network_interface = os.environ['SM_NETWORK_INTERFACE_NAME']
  ```
+ `SM_USER_ARGS`

  ```
  SM_USER_ARGS='["--batch-size","256","--learning_rate","0.0001","--communicator","pure_nccl"]'
  ```

  The `SM_INPUT_DIR` environment variable contains a JSON\-encoded list of the script arguments provided for training\.
+ `SM_INPUT_DIR`

  ```
  SM_INPUT_DIR=/opt/ml/input/
  ```

  The `SM_INPUT_DIR` environment variable contains the path of the input directory, `/opt/ml/input/`\. This is the directory where Amazon SageMaker saves input data and configuration files before and during training\.
+ `SM_INPUT_CONFIG_DIR`

  ```
  SM_INPUT_DIR=/opt/ml/input/config
  ```

  The `SM_INPUT_CONFIG_DIR` environment variable contains the path of the input config directory, `/opt/ml/input/config/`\. This is the directory where standard Amazon SageMaker configuration files are located\.

  When training starts, Amazon SageMaker creates the following files in this directory:
  + `hyperparameters.json` – Contains the hyperparameters specified in the `CreateTrainingJob` request\. 
  + `inputdataconfig.json` – Contains the data channel information that you specified in the `InputDataConfig` parameter in a `CreateTrainingJob` request\.
  + `resourceconfig.json` – Contains the name of the current host and all host containers used in the training\.

  For more information, see: [Using Your Own Training Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo.html)\.
+ `SM_OUTPUT_DATA_DIR`

  ```
  SM_OUTPUT_DATA_DIR=/opt/ml/output/data/algo-1
  ```

  The `SM_OUTPUT_DATA_DIR` environment variable contains the directory where the algorithm writes non\-model training artifacts, such as evaluation results\. Amazon SageMaker retains these artifacts\. As it runs in a container, your algorithm generates output, including the status of the training job and model, and the output artifacts\. Your algorithm should write this information to this directory\.
+ `SM_RESOURCE_CONFIG`

  ```
  SM_RESOURCE_CONFIG='{"current_host":"algo-1","hosts":["algo-1","algo-2"]}'
  ```

  The `SM_RESOURCE_CONFIG` environment variable contains the contents of the `resourceconfig.json` file located in the `/opt/ml/input/config/` directory\. It has the following keys:
  + `current_host` – The name of the current container on the container network\. For example, `"algo-1"`\.
  + `hosts` – The list of names of all of the containers on the container network, sorted lexicographically\. For example, `["algo-1", "algo-2", "algo-3"]` for a three\-node cluster\.

  For more information about the `resourceconfig.json` file, see: [Distributed Training Configuration](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo.html#your-algorithms-training-algo-running-container-dist-training)\.
+ `SM_INPUT_DATA_CONFIG`

  ```
  SM_INPUT_DATA_CONFIG='{
      "testing": {
          "RecordWrapperType": "None",
          "S3DistributionType": "FullyReplicated",
          "TrainingInputMode": "File"
      },
      "training": {
          "RecordWrapperType": "None",
          "S3DistributionType": "FullyReplicated",
          "TrainingInputMode": "File"
      }
  }'
  ```

  The `SM_INPUT_DATA_CONFIG` environment variable contains the input data configuration of the `inputdataconfig.json` file located in the `/opt/ml/input/config/` directory\. 

  For more information about the `resourceconfig.json` file, see [Distributed Training Configuration](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo.html#your-algorithms-training-algo-running-container-dist-training)\.
+ `SM_TRAINING_ENV`

  ```
  SM_TRAINING_ENV='
  {
      "channel_input_dirs": {
          "test": "/opt/ml/input/data/testing",
          "train": "/opt/ml/input/data/training"
      },
      "current_host": "algo-1",
      "framework_module": "sagemaker_chainer_container.training:main",
      "hosts": [
          "algo-1",
          "algo-2"
      ],
      "hyperparameters": {
          "batch-size": 10000,
          "epochs": 1
      },
      "input_config_dir": "/opt/ml/input/config",
      "input_data_config": {
          "test": {
              "RecordWrapperType": "None",
              "S3DistributionType": "FullyReplicated",
              "TrainingInputMode": "File"
          },
          "train": {
              "RecordWrapperType": "None",
              "S3DistributionType": "FullyReplicated",
              "TrainingInputMode": "File"
          }
      },
      "input_dir": "/opt/ml/input",
      "job_name": "preprod-chainer-2018-05-31-06-27-15-511",
      "log_level": 20,
      "model_dir": "/opt/ml/model",
      "module_dir": "s3://sagemaker-{aws-region}-{aws-id}/{training-job-name}/source/sourcedir.tar.gz",
      "module_name": "user_script",
      "network_interface_name": "ethwe",
      "num_cpus": 4,
      "num_gpus": 1,
      "output_data_dir": "/opt/ml/output/data/algo-1",
      "output_dir": "/opt/ml/output",
      "resource_config": {
          "current_host": "algo-1",
          "hosts": [
              "algo-1",
              "algo-2"
          ]
      }
  }'
  ```

  The `SM_TRAINING_ENV` environment variable provides all of the training information as a JSON\-encoded dictionary\.