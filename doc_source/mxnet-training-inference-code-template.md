# Writing Custom Apache MXNet Model Training and Inference Code<a name="mxnet-training-inference-code-template"></a>

To train a model on Amazon SageMaker using custom Apache MXNet code and deploy it on Amazon SageMaker, your code must implement the following training code interface and inference code interface\.

+ Implement the following training code interface:

  ```
  # ---------------------------------------------------------------------------- #
  # Training functions                                                           #
  # ---------------------------------------------------------------------------- #
  
  def train(
      hyperparameters,
      input_data_config,
      channel_input_dirs,
      output_data_dir,
      model_dir,
      num_gpus,
      num_cpus,
      hosts,
      current_host,
      **kwargs):
  
      """
      [Required]
  
      Runs Apache MXNet training. Amazon SageMaker calls this function with information
      about the training environment. When called, if this function returns an
      object, that object is passed to a save function.  The save function
      can be used to serialize the model to the Amazon SageMaker training job model
      directory.
  
      The **kwargs parameter can be used to absorb any Amazon SageMaker parameters that
      your training job doesn't need to use. For example, if your training job
      doesn't need to know anything about the training environment, your function
      signature can be as simple as train(**kwargs).
  
      Amazon SageMaker invokes your train function with the following python kwargs:
  
      Args:
          - hyperparameters: The Amazon SageMaker Hyperparameters dictionary. A dict
              of string to string.
          - input_data_config: The Amazon SageMaker input channel configuration for
              this job.
          - channel_input_dirs: A dict of string-to-string maps from the
              Amazon SageMaker algorithm input channel name to the directory containing
              files for that input channel. Note, if the Amazon SageMaker training job
              is run in PIPE mode, this dictionary will be empty.
          - output_data_dir:
              The Amazon SageMaker output data directory. After the function returns, data written to this
              directory is made available in the Amazon SageMaker training job
              output location.
          - model_dir: The Amazon SageMaker model directory. After the function returns, data written to this
              directory is made available to the Amazon SageMaker training job
              model location.
          - num_gpus: The number of GPU devices available on the host this script
              is being executed on.
          - num_cpus: The number of CPU devices available on the host this script
              is being executed on.
          - hosts: A list of hostnames in the Amazon SageMaker training job cluster.
          - current_host: This host's name. It will exist in the hosts list.
          - kwargs: Other keyword args.
  
      Returns:
          - (object): Optional. An Apache MXNet model to be passed to the model
              save function. If you do not return anything (or return None),
              the save function is not called.
      """
      pass
  
  def save(model, model_dir):
      """
      [Optional]
  
      Saves an Apache MXNet model after training. This function is called with the
      return value of train, if there is one. You are free to implement this to
      perform your own saving operation.
  
      Amazon SageMaker provides a default save function for
      Apache MXNet models. The default save function serializes 'Apache MXNet
      Module <https://mxnet.incubator.apache.org/api/python/module.html>' models.
      To rely on the default save function, omit a definition of
      'save' from your script. The default save function is discussed in more
      detail in the Amazon SageMaker Python SDK GitHub documentation.
  
      If you are using the Gluon API, you should provide your own save function,
      or save your model in the train function and let the train function
      complete without returning anything.
  
      Arguments:
         - model (object): The return value from train.
         - model_dir: The Amazon SageMaker model directory.
      """
      pass
  ```

+ Implement the following inference code interface:

  ```
  # ---------------------------------------------------------------------------- #
  # Hosting functions                                                            #
  # ---------------------------------------------------------------------------- #
  
  
  def model_fn(model_dir):
  
      """
      [Optional]
  
      Loads a model from disk, reading from model_dir. Called once by each
      inference service worker when it is started.
  
      If you want to take advantage of Amazon SageMaker's default request handling
      functions, the returned object should be a `Gluon Block
      <https://mxnet.incubator.apache.org/api/python/gluon/gluon.html#mxnet.gluon.Block>
      or MXNet `Module <https://mxnet.incubator.apache.org/api/python/module.html>`,
       described below. If you are providing your own transform_fn,
      then your model_fn can return anything that is compatible with your
       transform_fn.
  
      Amazon SageMaker provides a default model_fn that works with the serialization
      format used by the Amazon SageMaker default save function, discussed above. If
      you saved your model using the default save function, you do not need to
      provide a model_fn in your hosting script.
  
      Args:
          - model_dir: The Amazon SageMaker model directory.
  
      Returns:
          - (object): Optional. The deserialized model.
      """
      pass
  
  
  def transform_fn(model, input_data, content_type, accept):
      """
      [Optional]
  
      Transforms input data into a prediction result. Amazon SageMaker invokes your
      transform_fn in response to an InvokeEndpoint operation on an Amazon SageMaker
      endpoint containing this script. Amazon SageMaker passes in the model, previously
      loaded with model_fn, along with the input data, request content type, and accept content type from the InvokeEndpoint request.
  
      The input data is expected to have the given content_type.
  
      The output returned should have the given accept content type.
  
      This function should return a tuple of (transformation result, content
      type). In most cases, the returned content type should be the same as the
      accept content type, but it might differ. For example, when your code needs to
      return an error response.
  
      If you provide a transform_fn in your hosting script, it will be used
      to handle the entire request. You don't need to provide any other
      request handling functions (input_fn, predict_fn, or output_fn).
      If you do provide them, they will be ignored.
  
      Amazon SageMaker provides default transform_fn implementations that work with
      Gluon and Module models. These support JSON input and output, and for Module
      models, also CSV. To use the default transform_fn, provide a
      hosting script without a transform_fn or any other request handling
      functions. For more information about the default transform_fn,
      see the SageMaker Python SDK GitHub documentation. 
  
      Args:
          - input_data: The input data from the payload of the
              InvokeEndpoint request.
          - content_type: The content type of the request.
          - accept: The content type from the request's Accept header.
  
      Returns:
          - (object, string): A tuple containing the transformed result
              and its content type
      """
      pass
  
  
  # ---------------------------------------------------------------------------- #
  # Request handlers for Gluon models                                            #
  # ---------------------------------------------------------------------------- #
  
  
  def input_fn(input_data, content_type):
      """
      [Optional]
  
      Prepares data for transformation. Amazon SageMaker invokes your input_fn in
      response to an InvokeEndpoint operation on an Amazon SageMaker endpoint that contains
      this script. Amazon SageMaker passes in input data and content type from the
      InvokeEndpoint request.
  
      The function should return an NDArray that can be passed to the
      predict_fn.
  
      If you omit this function, Amazon SageMaker provides a default implementation.
      The default input_fn converts JSON-encoded array data into an NDArray.
      For more information about the default input_fn, see the SageMaker
      Python SDK GitHub documentation. 
  
      Args:
          - input_data: The input data from the payload of the
              InvokeEndpoint request.
          - content_type: The content type of the request.
  
      Returns:
          - (NDArray): An NDArray
      """
      pass
  
  
  def predict_fn(block, array):
      """
      [Optional]
  
      Performs prediction on an NDArray object. In response to an InvokeEndpoint request, Amazon SageMaker invokes your
      predict_fn with the model returned by your model_fn and the result
      of the input_fn.
  
      The function should return an NDArray containing the prediction results.
  
      If you omit this function, Amazon SageMaker provides a default implementation.
      The default predict_fn call passes the array to the forward
      method of the Gluon block, for example, block(array).
  
      Args:
          - block (Block): The loaded Gluon model; the result of calling
              model_fn on this script.
          - array (NDArray): The result of a call to input_fn in response
              to an Amazon SageMaker InvokeEndpoint request.
  
      Returns:
          - (NDArray): An NDArray containing the prediction result.
      """
      pass
  
  
  def output_fn(ndarray, accept):
      """
      [Optional]
  
      Serializes prediction results. Amazon SageMaker invokes your output_fn with
      the NDArray returned by your predict_fn and the content type from the
      InvokeEndpoint request's accept header.
  
      This function should return a tuple of (transformation result, content
      type). In most cases, the returned content type should be the same as the
      accept content type, but it might differ; for example, when your code needs to
      return an error response.
  
      If you omit this function, Amazon SageMaker provides a default implementation.
      The default output_fn converts the prediction result into a JSON-encoded
      array data. For more information about the default input_fn, see the
      Amazon SageMaker Python SDK GitHub documentation. 
  
      Args:
          - ndarray: NDArray. The result of calling predict_fn.
          - content_type: string. The content type from the InvokeEndpoint
              request's Accept header.
  
      Returns:
          - (object, string): A tuple containing the transformed result
              and its content type
      """
      pass
  
  
  # ---------------------------------------------------------------------------- #
  # Request handlers for Module models                                           #
  # ---------------------------------------------------------------------------- #
  
  def input_fn(model, input_data, content_type):
      """
      [Optional]
  
      Prepares data for transformation. Amazon SageMaker invokes your input_fn in
      response to an InvokeEndpoint operation on an Amazon SageMaker endpoint that contains
      this script. Amazon SageMaker passes in the MXNet Module returned by your
      model_fn, along with the input data and content type from the
      InvokeEndpoint request.
  
      The function should return an NDArray. Amazon SageMaker wraps the returned
      NDArray in a DataIter with a batch size that matches your model, and then
      passes it to your predict_fn.
  
      If you omit this function, Amazon SageMaker provides a default implementation.
      The default input_fn converts a JSON or CSV-encoded array data into an
      NDArray. For more information about the default input_fn, see the
      Amazon SageMaker Python SDK GitHub documentation. 
  
      Args:
          - model: A Module; the result of calling model_fn on this script.
          - input_data: The input data from the payload of the
              InvokeEndpoint request.
          - content_type: The content type of the request.
  
      Returns:
          - (NDArray): an NDArray
      """
      pass
  
  
  def predict_fn(module, data):
      """
      [Optional]
  
      Performs prediction on an MXNet DataIter object. In response to an
      InvokeEndpoint request, Amazon SageMaker invokes your
      predict_fn with the model returned by your model_fn and DataIter
      that contains the result of the input_fn.
  
      The function should return a list of NDArray or a list of list of NDArray
      containing the prediction results. For more information, see the MXNet Module API
      <https://mxnet.incubator.apache.org/api/python/module.html#mxnet.module.BaseModule.predict>.
  
      If you omit this function, Amazon SageMaker provides a default implementation.
      The default predict_fn calls module.predict on the input
      data and returns the result.
  
      Args:
          - module (Module): The loaded MXNet Module; the result of calling
              model_fn on this script.
          - data (DataIter): A DataIter containing the results of a
              call to input_fn.
  
      Returns:
          - (object): A list of NDArray or list of list of NDArray
              containing the prediction results.
      """
      pass
  
  
  def output_fn(data, accept):
      """
      [Optional]
  
      Serializes prediction results. Amazon SageMaker invokes your output_fn with the
      results of predict_fn and the content type from the InvokeEndpoint
      request's accept header.
  
      This function should return a tuple of (transformation result, content
      type). In most cases, the returned content type should be the same as the
      accept content type, but it might differ. For example, when your code needs to
      return an error response.
  
      If you omit this function, Amazon SageMaker provides a default implementation.
      The default output_fn converts the prediction result into JSON or CSV-
      encoded array data, depending on the value of the accept header. For more
      information about the default input_fn, see the Amazon SageMaker Python SDK
      GitHub documentation. 
  
      Args:
          - data: A list of NDArray or list of list of NDArray. The result of
              calling predict_fn.
          - content_type: A string. The content type from the InvokeEndpoint
              request's Accept header.
  
      Returns:
          - (object, string): A tuple containing the transformed result
              and its content type.
      """
      pass
  ```