# Environmental Variables used by Amazon SageMaker Containers Important for Running User Scripts<a name="docker-container-environmental-variables-user-scripts"></a>

When you write a script to run in a container, you are likely to use the following build\-time environment variables\. [Amazon SageMaker Containers](https://github.com/aws/sagemaker-containers) sets some of these variable values by default\.
+ `SM_MODEL_DIR`

  ```
  SM_MODEL_DIR=/opt/ml/model
  ```

  When the training job finishes, Amazon SageMaker deletes the container, including its file system, except for the files in the `/opt/ml/model` and `/opt/ml/output` folders\. Use `/opt/ml/mode`l to save the model checkpoints\. Amazon SageMaker uploads these checkpoints to the default S3 bucket\. Examples:

  ```
  # Using it in argparse
  parser.add_argument('model_dir', type=str, default=os.environ['SM_MODEL_DIR'])
  
  # Using it as a variable
  model_dir = os.environ['SM_MODEL_DIR']
  
  # Saving checkpoints to the model directory in Chainer
  serializers.save_npz(os.path.join(os.environ['SM_MODEL_DIR'], 'model.npz'), model)
  ```

  For more information, see [How Amazon SageMaker Processes Training Output](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo.html#your-algorithms-training-algo-envvariables)\.
+ `SM_CHANNELS`

  ```
  SM_CHANNELS='["testing","training"]'
  ```

  The `SM_CHANNELS` environmental variable contains the list of input data channels for the container\. When you train a model, you can partition your training data into different logical "channels"\. Common channels are: training, testing,and evaluation, or images and labels\. `SM_CHANNELS` includes the name of the channels that are in the container as a JSON encoded list\. 

  Examples:

  ```
  import json
  
  # Using it in argparse
  parser.add_argument('channel_names', type=int, default=json.loads(os.environ['SM_CHANNELS'])))
  
  # Using it as a variable
  channel_names = json.loads(os.environ['SM_CHANNELS']))
  ```
+ `SM_CHANNEL_{channel_name}`

  ```
  SM_CHANNEL_TRAINING='/opt/ml/input/data/training'
  SM_CHANNEL_TESTING='/opt/ml/input/data/testing'
  ```

  The `SM_CHANNEL_{channel_name}` environmental variable contains the directory where the channel named `channel_name` is located in the container\. 

  Examples:

  ```
  import json
  
  parser.add_argument('--train', type=str, default=os.environ['SM_CHANNEL_TRAINING'])
  parser.add_argument('--test', type=str, default=os.environ['SM_CHANNEL_TESTING'])
  
  
  args = parser.parse_args()
  
  train_file = np.load(os.path.join(args.train, 'train.npz'))
  test_file = np.load(os.path.join(args.test, 'test.npz'))
  ```
+ `SM_HPS`

  ```
  SM_HPS='{"batch-size": "256", "learning-rate": "0.0001","communicator": "pure_nccl"}'
  ```

  The `SM_HPS` environmental variable contains a JSON encoded dictionary with the hyperparameters that you have provided\. 

  Example:

  ```
  import json
  
  hyperparameters = json.loads(os.environ['SM_HPS']))
  # {"batch-size": 256, "learning-rate": 0.0001, "communicator": "pure_nccl"}
  ```
+ `SM_HP_ {hyperparameter_name}`

  ```
  SM_HP_LEARNING-RATE=0.0001
  SM_HP_BATCH-SIZE=10000
  SM_HP_COMMUNICATOR=pure_nccl
  ```

  The `SM_HP_ {hyperparameter_name` environmental variable contains the value of the hyperparameter named `hyperparameter_name`\. 

  Examples:

  ```
  learning_rate = float(os.environ['SM_HP_LEARNING-RATE'])
  batch_size = int(os.environ['SM_HP_BATCH-SIZE'])
  comminicator = os.environ['SM_HP_COMMUNICATOR']
  ```
+ `SM_CURRENT_HOST`

  ```
  SM_CURRENT_HOST=algo-1
  ```

  The `SM_CURRENT HOST` contains the name of the current container on the container network\. 

  Examples:

  ```
  # Using it in argparse
  parser.add_argument('current_host', type=str, default=os.environ['SM_CURRENT_HOST'])
  
  # Using it as a variable
  current_host = os.environ['SM_CURRENT_HOST']
  ```
+ `SM_HOSTS`

  ```
  SM_HOSTS='["algo-1","algo-2"]'
  ```

  The `SM_HOSTS` environmental variable contains a JSON\-encoded list of all of the hosts\. 

  Example:

  ```
  import json
  
  # Using it in argparse
  parser.add_argument('hosts', type=nargs, default=json.loads(os.environ['SM_HOSTS']))
  
  # Using it as variable
  hosts = json.loads(os.environ['SM_HOSTS'])
  ```
+ `SM_NUM_GPUS`

  ```
  SM_NUM_GPUS=1
  ```

  The `SM_NUM_GPUS` environmental variable contains the number of GPUs available in the current container\. 

  Examples:

  ```
  # Using it in argparse
  parser.add_argument('num_gpus', type=int, default=os.environ['SM_NUM_GPUS'])
  
  # Using it as a variable
  num_gpus = int(os.environ['SM_NUM_GPUS'])
  ```