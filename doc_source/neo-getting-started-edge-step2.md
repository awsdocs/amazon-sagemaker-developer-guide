# Step 2: Set Up Your Device<a name="neo-getting-started-edge-step2"></a>

You will need to install packages on your edge device so that your device can make inferences\. You will also need to either install [AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/latest/developerguide/what-is-gg.html) core or [Deep Learning Runtime \(DLR\)](https://github.com/neo-ai/neo-ai-dlr)\. In this example, you will install packages required to make inferences for the `coco_ssd_mobilenet` object detection algorithm and you will use DLR\.

1. **Install additional packages**

   In addition to Boto3, you must install certain libraries on your edge device\. What libraries you install depends on your use case\. 

   For example, for the `coco_ssd_mobilenet` object detection algorithm you downloaded earlier, you need to install [NumPy](https://numpy.org/) for data manipulation and statistics, [PIL](https://pillow.readthedocs.io/en/stable/) to load images, and [Matplotlib](https://matplotlib.org/) to generate plots\. You also need a copy of TensorFlow if you want to gauge the impact of compiling with Neo versus a baseline\. 

   ```
   !pip3 install numpy pillow tensorflow matplotlib 
   ```

1. **Install inference engine on your device**

   To run your Neo\-compiled model, install the [Deep Learning Runtime \(DLR\)](https://github.com/neo-ai/neo-ai-dlr) on your device\. DLR is a compact, common runtime for deep learning models and decision tree models\. On x86\_64 CPU targets running Linux, you can install the latest release of the DLR package using the following `pip` command:

   ```
   !pip install dlr
   ```

   For installation of DLR on GPU targets or non\-x86 edge devices, refer to [Releases](https://github.com/neo-ai/neo-ai-dlr/releases) for prebuilt binaries, or [Installing DLR](https://neo-ai-dlr.readthedocs.io/en/latest/install.html) for building DLR from source\. For example, to install DLR for Raspberry Pi 3, you can use: 

   ```
   !pip install https://neo-ai-dlr-release.s3-us-west-2.amazonaws.com/v1.3.0/pi-armv7l-raspbian4.14.71-glibc2_24-libstdcpp3_4/dlr-1.3.0-py3-none-any.whl
   ```