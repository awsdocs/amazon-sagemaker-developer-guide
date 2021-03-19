# Step 3: Make Inferences on Your Device<a name="neo-getting-started-edge-step3"></a>

In this example, you will use Boto3 to download the output of your compilation job onto your edge device\. You will then import DLR, download an example images from the dataset, resize this image to match the model’s original input, and then you will make a prediction\.

1. **Download your compiled model from Amazon S3 to your device and extract it from the compressed tarfile\.** 

   ```
   # Download compiled model locally to edge device
   object_path = f'output/{model_name}-{target_device}.tar.gz'
   neo_compiled_model = f'compiled-{model_name}.tar.gz'
   s3_client.download_file(bucket_name, object_path, neo_compiled_model)
   
   # Extract model from .tar.gz so DLR can use it
   !mkdir ./dlr_model # make a directory to store your model (optional)
   !tar -xzvf ./compiled-detect.tar.gz --directory ./dlr_model
   ```

1. **Import DLR and an initialized `DLRModel` object\.**

   ```
   import dlr
   
   device = 'cpu'
   model = dlr.DLRModel('./dlr_model', device)
   ```

1. **Download an image for inferencing and format it based on how your model was trained**\.

   For the `coco_ssd_mobilenet` example, you can download an image from the [COCO dataset](https://cocodataset.org/#home) and then reform the image to `300x300`: 

   ```
   from PIL import Image
   
   # Download an image for model to make a prediction
   input_image_filename = './input_image.jpg'
   !curl https://farm9.staticflickr.com/8325/8077197378_79efb4805e_z.jpg --output {input_image_filename}
   
   # Format image so model can make predictions
   resized_image = image.resize((300, 300))
   
   # Model is quantized, so convert the image to uint8
   x = np.array(resized_image).astype('uint8')
   ```

1. **Use DLR to make inferences**\.

   Finally, you can use DLR to make a prediction on the image you just downloaded: 

   ```
   out = model.run(x)
   ```

For more examples using DLR to make inferences from a Neo\-compiled model on an edge device, see the [neo\-ai\-dlr Github repository](https://github.com/neo-ai/neo-ai-dlr)\. 