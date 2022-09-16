# Manage Model<a name="edge-manage-model"></a>

The Edge Manager agent can load multiple models at a time and make inference with loaded models on edge devices\. The number of models the agent can load is determined by the available memory on the device\. The agent validates the model signature and loads into memory all the artifacts produced by the edge packaging job\. This step requires all the required certificates described in previous steps to be installed along with rest of the binary installation\. If the model’s signature cannot be validated, then loading of the model fails with appropriate return code and reason\.

SageMaker Edge Manager agent provides a list of Model Management APIs that implement control plane and data plane APIs on edge devices\. Along with this documentation, we recommend going through the sample client implementation which shows canonical usage of the below described APIs\.

The `proto` file is available as a part of the release artifacts \(inside the release tarball\)\. In this doc, we list and describe the usage of APIs listed in this `proto` file\.

**Note**  
There is one\-to\-one mapping for these APIs on Windows release and a sample code for an application implement in C\# is shared with the release artifacts for Windows\. Below instructions are for running the Agent as a standalone process, applicable for to the release artifacts for Linux\.

Extract the archive based on your OS\. Where `VERSION` is broken into three components: `<MAJOR_VERSION>.<YYYY-MM-DD>-<SHA-7>`\. See [Installing the Edge Manager agent](edge-device-fleet-manual.md#edge-device-fleet-installation) for information on how to obtain the release version \(`<MAJOR_VERSION>`\), time stamp of the release artifact \(`<YYYY-MM-DD>`\), and the repository commit ID \(`SHA-7`\)

------
#### [ Linux ]

The zip archive can be extracted with the command:

```
tar -xvzf <VERSION>.tgz
```

------
#### [ Windows ]

The zip archive can be extracted with the UI or command:

```
unzip <VERSION>.tgz
```

------

The release artifact hierarchy \(after extracting the `tar/zip` archive\) is shown below\. The agent `proto` file is available under `api/`\.

```
0.20201205.7ee4b0b
├── bin
│         ├── sagemaker_edge_agent_binary
│         └── sagemaker_edge_agent_client_example
└── docs
├── api
│         └── agent.proto
├── attributions
│         ├── agent.txt
│         └── core.txt
└── examples
└── ipc_example
├── CMakeLists.txt
├── sagemaker_edge_client.cc
├── sagemaker_edge_client_example.cc
├── sagemaker_edge_client.hh
├── sagemaker_edge.proto
├── README.md
├── shm.cc
├── shm.hh
└── street_small.bmp
```

**Topics**
+ [Load Model](#edge-manage-model-loadmodel)
+ [Unload Model](#edge-manage-model-unloadmodel)
+ [List Models](#edge-manage-model-listmodels)
+ [Describe Model](#edge-manage-model-describemodel)
+ [Capture Data](#edge-manage-model-capturedata)
+ [Get Capture Status](#edge-manage-model-getcapturedata)
+ [Predict](#edge-manage-model-predict)

## Load Model<a name="edge-manage-model-loadmodel"></a>

The Edge Manager agent supports loading multiple models\. This API validates the model signature and loads into memory all the artifacts produced by the `EdgePackagingJob` operation\. This step requires all the required certificates to be installed along with rest of the agent binary installation\. If the model’s signature cannot be validated then this step fails with appropriate return code and error messages in the log\.

```
// perform load for a model
// Note:
// 1. currently only local filesystem paths are supported for loading models.
// 2. multiple models can be loaded at the same time, as limited by available device memory
// 3. users are required to unload any loaded model to load another model.
// Status Codes:
// 1. OK - load is successful
// 2. UNKNOWN - unknown error has occurred
// 3. INTERNAL - an internal error has occurred
// 4. NOT_FOUND - model doesn't exist at the url
// 5. ALREADY_EXISTS - model with the same name is already loaded
// 6. RESOURCE_EXHAUSTED - memory is not available to load the model
// 7. FAILED_PRECONDITION - model is not compiled for the machine.
//
rpc LoadModel(LoadModelRequest) returns (LoadModelResponse);
```

------
#### [ Input ]

```
//
// request for LoadModel rpc call
//
message LoadModelRequest {
  string url = 1;
  string name = 2;  // Model name needs to match regex "^[a-zA-Z0-9](-*[a-zA-Z0-9])*$"
}
```

------
#### [ Output ]

```
//
//
// response for LoadModel rpc call
//
message LoadModelResponse {
  Model model = 1;
}

//
// Model represents the metadata of a model
//  url - url representing the path of the model
//  name - name of model
//  input_tensor_metadatas - TensorMetadata array for the input tensors
//  output_tensor_metadatas - TensorMetadata array for the output tensors
//
// Note:
//  1. input and output tensor metadata could empty for dynamic models.
//
message Model {
  string url = 1;
  string name = 2;
  repeated TensorMetadata input_tensor_metadatas = 3;
  repeated TensorMetadata output_tensor_metadatas = 4;
}
```

------

## Unload Model<a name="edge-manage-model-unloadmodel"></a>

Unloads a previously loaded model\. It is identified via the model alias which was provided during `loadModel`\. If the alias is not found or model is not loaded then returns error\.

```
//
// perform unload for a model
// Status Codes:
// 1. OK - unload is successful
// 2. UNKNOWN - unknown error has occurred
// 3. INTERNAL - an internal error has occurred
// 4. NOT_FOUND - model doesn't exist
//
rpc UnLoadModel(UnLoadModelRequest) returns (UnLoadModelResponse);
```

------
#### [ Input ]

```
//
// request for UnLoadModel rpc call
//
message UnLoadModelRequest {
 string name = 1; // Model name needs to match regex "^[a-zA-Z0-9](-*[a-zA-Z0-9])*$"
}
```

------
#### [ Output ]

```
//
// response for UnLoadModel rpc call
//
message UnLoadModelResponse {}
```

------

## List Models<a name="edge-manage-model-listmodels"></a>

Lists all the loaded models and their aliases\.

```
//
// lists the loaded models
// Status Codes:
// 1. OK - unload is successful
// 2. UNKNOWN - unknown error has occurred
// 3. INTERNAL - an internal error has occurred
//
rpc ListModels(ListModelsRequest) returns (ListModelsResponse);
```

------
#### [ Input ]

```
//
// request for ListModels rpc call
//
message ListModelsRequest {}
```

------
#### [ Output ]

```
//
// response for ListModels rpc call
//
message ListModelsResponse {
 repeated Model models = 1;
}
```

------

## Describe Model<a name="edge-manage-model-describemodel"></a>

Describes a model that is loaded on the agent\.

```
//
// Status Codes:
// 1. OK - load is successful
// 2. UNKNOWN - unknown error has occurred
// 3. INTERNAL - an internal error has occurred
// 4. NOT_FOUND - model doesn't exist at the url
//
rpc DescribeModel(DescribeModelRequest) returns (DescribeModelResponse);
```

------
#### [ Input ]

```
//
// request for DescribeModel rpc call
//
message DescribeModelRequest {
  string name = 1;
}
```

------
#### [ Output ]

```
//
// response for DescribeModel rpc call
//
message DescribeModelResponse {
  Model model = 1;
}
```

------

## Capture Data<a name="edge-manage-model-capturedata"></a>

Allows the client application to capture input and output tensors in Amazon S3 bucket, and optionally the auxiliary\. The client application is expected to pass a unique capture ID along with each call to this API\. This can be later used to query status of the capture\.

```
//
// allows users to capture input and output tensors along with auxiliary data.
// Status Codes:
// 1. OK - data capture successfully initiated
// 2. UNKNOWN - unknown error has occurred
// 3. INTERNAL - an internal error has occurred
// 5. ALREADY_EXISTS - capture initiated for the given capture_id
// 6. RESOURCE_EXHAUSTED - buffer is full cannot accept any more requests.
// 7. OUT_OF_RANGE - timestamp is in the future.
// 8. INVALID_ARGUMENT - capture_id is not of expected format.
//
rpc CaptureData(CaptureDataRequest) returns (CaptureDataResponse);
```

------
#### [ Input ]

```
enum Encoding {
 CSV = 0;
 JSON = 1;
 NONE = 2;
 BASE64 = 3;
}

//
// AuxilaryData represents a payload of extra data to be capture along with inputs and outputs of inference
// encoding - supports the encoding of the data
// data - represents the data of shared memory, this could be passed in two ways:
// a. send across the raw bytes of the multi-dimensional tensor array
// b. send a SharedMemoryHandle which contains the posix shared memory segment id and
// offset in bytes to location of multi-dimensional tensor array.
//
message AuxilaryData {
 string name = 1;
 Encoding encoding = 2;
 oneof data {
 bytes byte_data = 3;
 SharedMemoryHandle shared_memory_handle = 4;
 }
}

//
// Tensor represents a tensor, encoded as contiguous multi-dimensional array.
// tensor_metadata - represents metadata of the shared memory segment
// data_or_handle - represents the data of shared memory, this could be passed in two ways:
// a. send across the raw bytes of the multi-dimensional tensor array
// b. send a SharedMemoryHandle which contains the posix shared memory segment
// id and offset in bytes to location of multi-dimensional tensor array.
//
message Tensor {
 TensorMetadata tensor_metadata = 1; //optional in the predict request
 oneof data {
 bytes byte_data = 4;
 // will only be used for input tensors
 SharedMemoryHandle shared_memory_handle = 5;
 }
}

//
// request for CaptureData rpc call
//
message CaptureDataRequest {
 string model_name = 1;
 string capture_id = 2; //uuid string
 Timestamp inference_timestamp = 3;
 repeated Tensor input_tensors = 4;
 repeated Tensor output_tensors = 5;
 repeated AuxilaryData inputs = 6;
 repeated AuxilaryData outputs = 7;
}
```

------
#### [ Output ]

```
//
// response for CaptureData rpc call
//
message CaptureDataResponse {}
```

------

## Get Capture Status<a name="edge-manage-model-getcapturedata"></a>

Depending on the models loaded the input and output tensors can be large \(for many edge devices\)\. Capture to the cloud can be time consuming\. So the `CaptureData()` is implemented as an asynchronous operation\. A capture ID is a unique identifier that the client provides during capture data call, this ID can be used to query the status of the asynchronous call\.

```
//
// allows users to query status of capture data operation
// Status Codes:
// 1. OK - data capture successfully initiated
// 2. UNKNOWN - unknown error has occurred
// 3. INTERNAL - an internal error has occurred
// 4. NOT_FOUND - given capture id doesn't exist.
//
rpc GetCaptureDataStatus(GetCaptureDataStatusRequest) returns (GetCaptureDataStatusResponse);
```

------
#### [ Input ]

```
//
// request for GetCaptureDataStatus rpc call
//
message GetCaptureDataStatusRequest {
  string capture_id = 1;
}
```

------
#### [ Output ]

```
enum CaptureDataStatus {
  FAILURE = 0;
  SUCCESS = 1;
  IN_PROGRESS = 2;
  NOT_FOUND = 3;
}

//
// response for GetCaptureDataStatus rpc call
//
message GetCaptureDataStatusResponse {
  CaptureDataStatus status = 1;
}
```

------

## Predict<a name="edge-manage-model-predict"></a>

The `predict` API performs inference on a previously loaded model\. It accepts a request in the form of a tensor that is directly fed into the neural network\. The output is the output tensor \(or scalar\) from the model\. This is a blocking call\.

```
//
// perform inference on a model.
//
// Note:
// 1. users can chose to send the tensor data in the protobuf message or
// through a shared memory segment on a per tensor basis, the Predict
// method with handle the decode transparently.
// 2. serializing large tensors into the protobuf message can be quite expensive,
// based on our measurements it is recommended to use shared memory of
// tenors larger than 256KB.
// 3. SMEdge IPC server will not use shared memory for returning output tensors,
// i.e., the output tensor data will always send in byte form encoded
// in the tensors of PredictResponse.
// 4. currently SMEdge IPC server cannot handle concurrent predict calls, all
// these call will be serialized under the hood. this shall be addressed
// in a later release.
// Status Codes:
// 1. OK - prediction is successful
// 2. UNKNOWN - unknown error has occurred
// 3. INTERNAL - an internal error has occurred
// 4. NOT_FOUND - when model not found
// 5. INVALID_ARGUMENT - when tenors types mismatch
//
rpc Predict(PredictRequest) returns (PredictResponse);
```

------
#### [ Input ]

```
// request for Predict rpc call
//
message PredictRequest {
string name = 1;
repeated Tensor tensors = 2;
}

//
// Tensor represents a tensor, encoded as contiguous multi-dimensional array.
//    tensor_metadata - represents metadata of the shared memory segment
//    data_or_handle - represents the data of shared memory, this could be passed in two ways:
//                        a. send across the raw bytes of the multi-dimensional tensor array
//                        b. send a SharedMemoryHandle which contains the posix shared memory segment
//                            id and offset in bytes to location of multi-dimensional tensor array.
//
message Tensor {
  TensorMetadata tensor_metadata = 1; //optional in the predict request
  oneof data {
    bytes byte_data = 4;
    // will only be used for input tensors
    SharedMemoryHandle shared_memory_handle = 5;
  }
}

//
// Tensor represents a tensor, encoded as contiguous multi-dimensional array.
//    tensor_metadata - represents metadata of the shared memory segment
//    data_or_handle - represents the data of shared memory, this could be passed in two ways:
//                        a. send across the raw bytes of the multi-dimensional tensor array
//                        b. send a SharedMemoryHandle which contains the posix shared memory segment
//                            id and offset in bytes to location of multi-dimensional tensor array.
//
message Tensor {
  TensorMetadata tensor_metadata = 1; //optional in the predict request
  oneof data {
    bytes byte_data = 4;
    // will only be used for input tensors
    SharedMemoryHandle shared_memory_handle = 5;
  }
}

//
// TensorMetadata represents the metadata for a tensor
//    name - name of the tensor
//    data_type  - data type of the tensor
//    shape - array of dimensions of the tensor
//
message TensorMetadata {
  string name = 1;
  DataType data_type = 2;
  repeated int32 shape = 3;
}

//
// SharedMemoryHandle represents a posix shared memory segment
//    offset - offset in bytes from the start of the shared memory segment.
//    segment_id - shared memory segment id corresponding to the posix shared memory segment.
//    size - size in bytes of shared memory segment to use from the offset position.
//
message SharedMemoryHandle {
  uint64 size = 1;
  uint64 offset = 2;
  uint64 segment_id = 3;
}
```

------
#### [ Output ]

**Note**  
The `PredictResponse` only returns `Tensors` and not `SharedMemoryHandle`\.

```
// response for Predict rpc call
//
message PredictResponse {
   repeated Tensor tensors = 1;
}
```

------