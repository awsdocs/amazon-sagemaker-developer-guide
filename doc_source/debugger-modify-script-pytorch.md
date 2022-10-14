# Adapt Your PyTorch Training Script<a name="debugger-modify-script-pytorch"></a>

To start collecting model output tensors and debug training issues, make the following modifications to your PyTorch training script\.

## For PyTorch 1\.12\.0<a name="debugger-modify-script-pytorch-1-12-0"></a>

If you bring a PyTorch training script, you can run the training job and extract model output tensors with a few additional code lines in your training script\. You need to use the [hook APIs](https://sagemaker-debugger.readthedocs.io/en/website/hook-api.html) in the `sagemaker-debugger` client library\. Walk through the following instructions that break down the steps with code examples\.

1. Create a hook\.

   **\(Recommended\) For training jobs within SageMaker**

   ```
   import smdebug.pytorch as smd
   hook = smd.get_hook(create_if_not_exists=True)
   ```

   When you launch a training job in [Step 2: Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration-for-debugging.md) with any of the DebuggerHookConfig, TensorBoardConfig, or Rules in your estimator, SageMaker adds a JSON configuration file to your training instance that is picked up by the `get_hook` function\. Note that if you do not include any of the configuration APIs in your estimator, there will be no configuration file for the hook to find, and the function returns `None`\.

   **\(Optional\) For training jobs outside SageMaker**

   If you run training jobs in local mode, directly running on SageMaker Notebook instances, Amazon EC2 instances, and your own local device, use `smd.Hook` class to directly create a hook\. However, this approach can only store the tensor collections and usable for TensorBoard visualization\. SageMaker Debugger’s built\-in Rules don’t work with the local mode\. The `smd.get_hook` method also returns `None` in this case\. 

   If you want to create a manual hook, use the following code snippet with the logic to check if the hook returns `None` and create a manual hook using the `smd.Hook` class\.

   ```
   import smdebug.pytorch as smd
   hook = smd.get_hook(create_if_not_exists=True)
   
   if hook is None:
       hook = smd.Hook(
           out_dir='/opt/ml/checkpoints/smdebugger',
           export_tensorboard=True
       )
   ```

1. Wrap your model with the hook’s class methods\.

   The `hook.register_module()` method takes your model and iterates through each layer, looking for any tensors that match with regular expressions that you’ll provide through the configuration in [Step 2: Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration-for-debugging.md)\. The collectable tensors through this hook method are weights, biases, activations, gradients, inputs, and outputs\.

   ```
   hook.register_module(model)
   ```
**Tip**  
If you collect the entire output tensors from a large deep learning model, the total size of those collections can exponentially grow and might cause bottlenecks\. If you want to save specific tensors, you can also use the `hook.save_tensor()` method\. This method helps you pick the variable for the specific tensor and save to a custom collection named as you want\. For more information, see [step 7](#debugger-modify-script-pytorch-save-custom-tensor) of this instruction\.

1. Warp the loss function with the hook’s class methods\.

   The `hook.register_loss` method is to wrap the loss function\. It extracts loss values every `save_interval` that you’ll set during configuration in [Step 2: Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration-for-debugging.md), and saves them to the `"losses"` collection\.

   ```
   hook.register_loss(loss_function)
   ```

1. Add `hook.set_mode(ModeKeys.TRAIN)` in the train block\. This indicates the tensor collection is extracted during the training phase\.

   ```
   def train():
       ...
       hook.set_mode(ModeKeys.TRAIN)
   ```

1. Add `hook.set_mode(ModeKeys.EVAL)` in the validation block\. This indicates the tensor collection is extracted during the validation phase\.

   ```
   def validation():
       ...
       hook.set_mode(ModeKeys.EVAL)
   ```

1. Use [https://sagemaker-debugger.readthedocs.io/en/website/hook-constructor.html#smdebug.core.hook.BaseHook.save_scalar](https://sagemaker-debugger.readthedocs.io/en/website/hook-constructor.html#smdebug.core.hook.BaseHook.save_scalar) to save custom scalars\. You can save scalar values that aren’t in your model\. For example, if you want to record the accuracy values computed during evaluation, add the following line of code below the line where you calculate accuracy\.

   ```
   hook.save_scalar("accuracy", accuracy)
   ```

   Note that you need to provide a string as the first argument to name the custom scalar collection\. This is the name that'll be used for visualizing the scalar values in TensorBoard, and can be any string you want\.

1. <a name="debugger-modify-script-pytorch-save-custom-tensor"></a>Use [https://sagemaker-debugger.readthedocs.io/en/website/hook-constructor.html#smdebug.core.hook.BaseHook.save_tensor](https://sagemaker-debugger.readthedocs.io/en/website/hook-constructor.html#smdebug.core.hook.BaseHook.save_tensor) to save custom tensors\. Similarly, you can save additional tensors, defining your own tensor collection\. For example, you can extract input image data that are passed into the model by adding the following code line, where the variable for the input image data is `image_inputs`\.

   ```
   hook.save_tensor("images", image_inputs)
   ```

   Also, note that you need to provide a string as the first argument to name the custom tensor collection\. 

After you have completed adapting your training script, proceed to [Step 2: Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration-for-debugging.md)\.