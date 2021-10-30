# Troubleshoot Ambarella Errors<a name="neo-troubleshooting-target-devices-ambarella"></a>

SageMaker Neo requires models to be packaged in a compressed TAR file \(`*.tar.gz`\)\. Ambarella devices require additional files to be included within the compressed TAR file before it is sent for compilation\. Include the following files within your compressed TAR file if you want to compile a model for Ambarella targets with SageMaker Neo:
+ A trained model using a framework supported by SageMaker Neo 
+ A JSON configuration file
+ Calibration images

For example, the contents of your compressed TAR file should look similar to the following example:

```
├──amba_config.json
├──calib_data
|    ├── data1
|    ├── data2
|    ├── .
|    ├── .
|    ├── .
|    └── data500
└──mobilenet_v1_1.0_0224_frozen.pb
```

The directory is configured as follows:
+ `amba_config.json` : Configuration file
+ `calib_data` : Folder containing calibration images
+ `mobilenet_v1_1.0_0224_frozen.pb` : TensorFlow model saved as a frozen graph

For information about frameworks supported by SageMaker Neo, see [Supported Frameworks](neo-supported-devices-edge-frameworks.md)\.

## Setting up the Configuration File<a name="neo-troubleshooting-target-devices-ambarella-config"></a>

The configuration file provides information required by the Ambarella toolchain to compile the model\. The configuration file must be saved as a JSON file and the name of the file must end with `*config.json`\. The following chart shows the contents of the configuration file\.


| Key | Description | Example | 
| --- | --- | --- | 
| inputs | Dictionary mapping input layers to attribute\. | <pre>{inputs:{"data":{...},"data1":{...}}}</pre> | 
| "data" | Input layer name\. Note: "data" is an example of the name you can use to label the input layer\. | "data" | 
| shape | Describes the shape of the input to the model\. This follows the same conventions that SageMaker Neo uses\. | "shape": "1,3,224,224" | 
| filepath | Relative path to the directory containing calibration images\. These can be binary or image files like JPG or PNG\. | "filepath": "calib\_data/" | 
| colorformat | Color format that model expects\. This will be used while converting images to binary\. Supported values: \[RGB, BGR\]\. Default is RGB\. | "colorformat":"RGB" | 
| mean | Mean value to be subtracted from the input\. Can be a single value or a list of values\. When the mean is given as a list the number of entries must match the channel dimension of the input\. | "mean":128\.0 | 
| scale | Scale value to be used for normalizing the input\. Can be a single value or a list of values\. When the scale is given as a list, the number of entries must match the channel dimension of the input\. | "scale": 255\.0 | 

The following is a sample configuration file: 

```
{
    "inputs": {
        "data": {
                "shape": "1, 3, 224, 224",
                "filepath": "calib_data/",
                "colorformat": "RGB",
                "mean":[128,128,128],
                "scale":[128.0,128.0,128.0]
        }
    }
}
```

## Calibration Images<a name="neo-troubleshooting-target-devices-ambarella-calibration-images"></a>

Quantize your trained model by providing calibration images\. Quantizing your model improves the the performance of the CVFlow engine on an Ambarella System on a Chip \(SoC\)\. The Ambarella toolchain uses the calibration images to determine how each layer in the model should be quantized to achieve optimal performance and accuracy\. Each layer is quantized independently to INT8 or INT16 formats\. The final model has a mix of INT8 and INT16 layers after quantization\.

**How many images should you use?**

It is recommended that you include between 100–200 images that are representative of the types of scenes the model is expected to handle\. The model compilation time increases linearly to the number of calibration images in the input file\.

**What are the recommended image formats?**

Calibration images can be in a raw binary format or image formats such as JPG and PNG\.

Your calibration folder can contain a mixture of images and binary files\. If the calibration folder contains both images and binary files, the toolchain first converts the images to binary files\. Once the conversion is complete, it uses the newly generated binary files along with the binary files that were originally in the folder\.

**Can I convert the images into binary format first?**

Yes\. You can convert the images to the binary format with open\-source packages such as [OpenCV](https://opencv.org/) or [PIL](https://python-pillow.org/)\. Crop and resize the images so they satisfy the input layer of your trained model\.



## Mean and Scale<a name="neo-troubleshooting-target-devices-ambarella-mean-scale"></a>

You can specify mean and scaling pre\-processing options to the Amberalla toolchain\. These operations are embedded into the network and are applied during inference on each input\. Do not provide processed data if you specify the mean or scale\. More specifically, do not provide data you have subtracted the mean from or have applied scaling to\.