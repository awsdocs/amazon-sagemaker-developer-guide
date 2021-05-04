# Tested Models<a name="neo-supported-edge-tested-models"></a>

The following collapsible sections provide information about machine learning models that were tested by the Amazon SageMaker Neo team\. Expand the collapsible section based on your framework to check if a model was tested\.

**Note**  
This is not a comprehensive list of models that can be compiled with Neo\.

See [Supported Frameworks](neo-supported-devices-edge-frameworks.md) and [SageMaker Neo Supported Operators](https://aws.amazon.com/releasenotes/sagemaker-neo-supported-frameworks-and-operators/) to find out if you can compile your model with SageMaker Neo\.

## DarkNet<a name="collapsible-section-1"></a>


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Alexnet |  |  |  |  |  |  |  |  |  | 
| Resnet50 | X | X |  | X | X | X |  | X | X | 
| YOLOv2 |  |  |  | X | X | X |  | X | X | 
| YOLOv2\_tiny | X | X |  | X | X | X |  | X | X | 
| YOLOv3\_416 |  |  |  | X | X | X |  | X | X | 
| YOLOv3\_tiny | X | X |  | X | X | X |  | X | X | 

## MXNet<a name="collapsible-section-2"></a>


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Alexnet |  |  | X |  |  |  |  |  |  | 
| Densenet121 |  |  | X |  |  |  |  |  |  | 
| DenseNet201 | X | X | X | X | X | X |  | X | X | 
| GoogLeNet | X | X |  | X | X | X |  | X | X | 
| InceptionV3 |  |  |  | X | X | X |  | X | X | 
| MobileNet0\.75 | X | X |  | X | X | X |  |  | X | 
| MobileNet1\.0 | X | X | X | X | X | X |  |  | X | 
| MobileNetV2\_0\.5 | X | X |  | X | X | X |  |  | X | 
| MobileNetV2\_1\.0 | X | X | X | X | X | X | X | X | X | 
| MobileNetV3\_Large | X | X | X | X | X | X | X | X | X | 
| MobileNetV3\_Small | X | X | X | X | X | X | X | X | X | 
| ResNeSt50 |  |  |  | X | X |  |  | X | X | 
| ResNet18\_v1 | X | X | X | X | X | X |  |  | X | 
| ResNet18\_v2 | X | X |  | X | X | X |  |  | X | 
| ResNet50\_v1 | X | X | X | X | X | X |  | X | X | 
| ResNet50\_v2 | X | X | X | X | X | X |  | X | X | 
| ResNext101\_32x4d |  |  |  |  |  |  |  |  |  | 
| ResNext50\_32x4d | X |  | X | X | X |  |  | X | X | 
| SENet\_154 |  |  |  | X | X | X |  | X | X | 
| SE\_ResNext50\_32x4d | X | X |  | X | X | X |  | X | X | 
| SqueezeNet1\.0 | X | X | X | X | X | X |  |  | X | 
| SqueezeNet1\.1 | X | X | X | X | X | X |  | X | X | 
| VGG11 | X | X | X | X | X |  |  | X | X | 
| Xception | X | X | X | X | X | X |  | X | X | 
| darknet53 | X | X |  | X | X | X |  | X | X | 
| resnet18\_v1b\_0\.89 | X | X |  | X | X | X |  |  | X | 
| resnet50\_v1d\_0\.11 | X | X |  | X | X | X |  |  | X | 
| resnet50\_v1d\_0\.86 | X | X | X | X | X | X |  | X | X | 
| ssd\_512\_mobilenet1\.0\_coco | X |  | X | X | X | X |  | X | X | 
| ssd\_512\_mobilenet1\.0\_voc | X |  | X | X | X | X |  | X | X | 
| ssd\_resnet50\_v1 | X |  | X | X | X |  |  | X | X | 
| yolo3\_darknet53\_coco | X |  |  | X | X |  |  | X | X | 
| yolo3\_mobilenet1\.0\_coco | X | X |  | X | X | X |  | X | X | 
| deeplab\_resnet50 |  |  | X |  |  |  |  |  |  | 

## Keras<a name="collapsible-section-3"></a>


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| densenet121 | X | X | X | X | X | X |  | X | X | 
| densenet201 | X | X | X | X | X | X |  |  | X | 
| inception\_v3 | X | X |  | X | X | X |  | X | X | 
| mobilenet\_v1 | X | X | X | X | X | X |  | X | X | 
| mobilenet\_v2 | X | X | X | X | X | X |  | X | X | 
| resnet152\_v1 |  |  |  | X | X |  |  |  | X | 
| resnet152\_v2 |  |  |  | X | X |  |  |  | X | 
| resnet50\_v1 | X | X | X | X | X |  |  | X | X | 
| resnet50\_v2 | X | X | X | X | X | X |  | X | X | 
| vgg16 |  |  | X | X | X |  |  | X | X | 

## ONNX<a name="collapsible-section-4"></a>


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| alexnet |  |  | X |  |  |  |  |  |  | 
| mobilenetv2\-1\.0 | X | X | X | X | X | X |  | X | X | 
| resnet18v1 | X |  |  | X | X |  |  |  | X | 
| resnet18v2 | X |  |  | X | X |  |  |  | X | 
| resnet50v1 | X |  | X | X | X |  |  | X | X | 
| resnet50v2 | X |  | X | X | X |  |  | X | X | 
| resnet152v1 |  |  |  | X | X | X |  |  | X | 
| resnet152v2 |  |  |  | X | X | X |  |  | X | 
| squeezenet1\.1 | X |  | X | X | X | X |  | X | X | 
| vgg19 |  |  | X |  |  |  |  |  | X | 

## PyTorch \(FP32\)<a name="collapsible-section-5"></a>


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| densenet121 | X | X |  | X | X | X |  | X | X | 
| inception\_v3 |  | X |  | X | X | X |  | X | X | 
| resnet152 |  |  |  | X | X | X |  |  | X | 
| resnet18 | X | X |  | X | X | X |  |  | X | 
| resnet50 | X | X |  | X | X |  |  | X | X | 
| squeezenet1\.0 | X | X |  | X | X | X |  |  | X | 
| squeezenet1\.1 | X | X |  | X | X | X |  | X | X | 

## TensorFlow<a name="collapsible-section-6"></a>

------
#### [ TensorFlow ]


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| densenet201 | X | X | X | X | X | X |  | X | X | 
| inception\_v3 | X | X | X | X | X | X |  | X | X | 
| mobilenet100\_v1 | X | X | X | X | X | X |  |  | X | 
| mobilenet100\_v2\.0 | X | X | X | X | X | X |  | X | X | 
| mobilenet130\_v2 | X | X |  | X | X | X |  |  | X | 
| mobilenet140\_v2 | X | X | X | X | X | X |  | X | X | 
| resnet50\_v1\.5 | X | X |  | X | X | X |  | X | X | 
| resnet50\_v2 | X | X | X | X | X | X |  | X | X | 
| squeezenet | X | X | X | X | X | X |  | X | X | 

------
#### [ TensorFlow\.Keras ]


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| DenseNet121  | X | X |  | X | X | X |  | X | X | 
| DenseNet201 | X | X |  | X | X | X |  |  | X | 
| InceptionV3 | X | X |  | X | X | X |  | X | X | 
| MobileNet | X | X |  | X | X | X |  | X | X | 
| MobileNetv2 | X | X |  | X | X | X |  | X | X | 
| NASNetLarge |  |  |  | X | X |  |  | X | X | 
| NASNetMobile | X | X |  | X | X | X |  | X | X | 
| ResNet101 |  |  |  | X | X | X |  |  | X | 
| ResNet101V2 |  |  |  | X | X | X |  |  | X | 
| ResNet152 |  |  |  | X | X |  |  |  | X | 
| ResNet152v2 |  |  |  | X | X |  |  |  | X | 
| ResNet50 | X | X |  | X | X |  |  | X | X | 
| ResNet50V2 | X | X |  | X | X | X |  | X | X | 
| VGG16 |  |  |  | X | X |  |  | X | X | 
| Xception | X | X |  | X | X | X |  | X | X | 

------

## TensorFlow\-Lite<a name="collapsible-section-7"></a>

------
#### [ TensorFlow\-Lite \(FP32\) ]


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| densenet\_2018\_04\_27 | X |  |  | X | X | X |  |  | X | 
| inception\_resnet\_v2\_2018\_04\_27 |  |  |  | X | X | X |  |  | X | 
| inception\_v3\_2018\_04\_27 |  |  |  | X | X | X |  |  | X | 
| inception\_v4\_2018\_04\_27 |  |  |  | X | X | X |  |  | X | 
| mnasnet\_0\.5\_224\_09\_07\_2018 | X |  |  | X | X | X |  |  | X | 
| mnasnet\_1\.0\_224\_09\_07\_2018 | X |  |  | X | X | X |  |  | X | 
| mnasnet\_1\.3\_224\_09\_07\_2018 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v1\_0\.25\_128 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v1\_0\.25\_224 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v1\_0\.5\_128 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v1\_0\.5\_224 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v1\_0\.75\_128 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v1\_0\.75\_224 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v1\_1\.0\_128 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v1\_1\.0\_192 | X |  |  | X | X | X |  |  | X | 
| mobilenet\_v2\_1\.0\_224 | X |  |  | X | X | X |  |  | X | 
| resnet\_v2\_101 |  |  |  | X | X | X |  |  | X | 
| squeezenet\_2018\_04\_27 | X |  |  | X | X | X |  |  | X | 

------
#### [ TensorFlow\-Lite \(INT8\) ]


| Models | ARM V8 | ARM Mali | Ambarella CV22 | Nvidia | Panorama | TI TDA4VM | Qualcomm QCS603 | X86\_Linux | X86\_Windows | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| inception\_v1 |  |  |  |  |  |  | X |  |  | 
| inception\_v2 |  |  |  |  |  |  | X |  |  | 
| inception\_v3 | X |  |  |  |  | X | X |  | X | 
| inception\_v4\_299 | X |  |  |  |  | X | X |  | X | 
| mobilenet\_v1\_0\.25\_128 | X |  |  |  |  | X |  |  | X | 
| mobilenet\_v1\_0\.25\_224 | X |  |  |  |  | X |  |  | X | 
| mobilenet\_v1\_0\.5\_128 | X |  |  |  |  | X |  |  | X | 
| mobilenet\_v1\_0\.5\_224 | X |  |  |  |  | X |  |  | X | 
| mobilenet\_v1\_0\.75\_128 | X |  |  |  |  | X |  |  | X | 
| mobilenet\_v1\_0\.75\_224 | X |  |  |  |  | X | X |  | X | 
| mobilenet\_v1\_1\.0\_128 | X |  |  |  |  | X |  |  | X | 
| mobilenet\_v1\_1\.0\_224 | X |  |  |  |  | X | X |  | X | 
| mobilenet\_v2\_1\.0\_224 | X |  |  |  |  | X | X |  | X | 
| deeplab\-v3\_513 |  |  |  |  |  |  | X |  |  | 

------