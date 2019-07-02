# Data Formats for Object2Vec Inference<a name="object2vec-inference-formats"></a>

## GPU optimization: Classification or Regression<a name="object2vec-inference-gpu-optimize-classification"></a>

Due to GPU memory scarcity, the `INFERENCE_PREFERRED_MODE` environment variable can be specified to optimize on whether the classification/regression or the [Output: Encoder Embeddings](object2vec-encoder-embeddings.md#object2vec-out-encoder-embeddings-data) inference network is loaded into GPU\. If the majority of your inference is for classification or regression, specify `INFERENCE_PREFERRED_MODE=classification`\. The following is a Batch Transform example of using 4 instances of p3\.2xlarge that optimizes for classification/regression inference:

```
transformer = o2v.transformer(instance_count=4, 
                              instance_type="ml.p2.xlarge", 
                              max_concurrent_transforms=2,
                              max_payload=1,  # 1MB
                              strategy='MultiRecord',
                              env={'INFERENCE_PREFERRED_MODE': 'classification'},  # only useful with GPU
                              output_path=output_s3_path)
```

## Input: Classification or Regression Request Format<a name="object2vec-in-inference-data"></a>

Content\-type: application/json

```
{
  "instances" : [
    {"in0": [6, 17, 606, 19, 53, 67, 52, 12, 5, 10, 15, 10178, 7, 33, 652, 80, 15, 69, 821, 4], "in1": [16, 21, 13, 45, 14, 9, 80, 59, 164, 4]},
    {"in0": [22, 1016, 32, 13, 25, 11, 5, 64, 573, 45, 5, 80, 15, 67, 21, 7, 9, 107, 4], "in1": [22, 32, 13, 25, 1016, 573, 3252, 4]},
    {"in0": [774, 14, 21, 206], "in1": [21, 366, 125]}
  ]
}
```

Content\-type: application/jsonlines

```
{"in0": [6, 17, 606, 19, 53, 67, 52, 12, 5, 10, 15, 10178, 7, 33, 652, 80, 15, 69, 821, 4], "in1": [16, 21, 13, 45, 14, 9, 80, 59, 164, 4]}
{"in0": [22, 1016, 32, 13, 25, 11, 5, 64, 573, 45, 5, 80, 15, 67, 21, 7, 9, 107, 4], "in1": [22, 32, 13, 25, 1016, 573, 3252, 4]}
{"in0": [774, 14, 21, 206], "in1": [21, 366, 125]}
```

For classification problems, the length of the scores vector corresponds to `num_classes`\. For regression problems, the length is 1\.

## Ouput: Classification or Regression Response Format<a name="object2vec-out-inference-data"></a>

Accept: application/json

```
{
    "predictions": [
        {
            "scores": [
                0.6533935070037842,
                0.07582679390907288,
                0.2707797586917877
            ]
        },       
        {
            "scores": [
                0.026291321963071823,
                0.6577019095420837,
                0.31600672006607056
            ]
        }
    ]
}
```

Accept: application/jsonlines

```
{"scores":[0.195667684078216,0.395351558923721,0.408980727195739]}
{"scores":[0.251988261938095,0.258233487606048,0.489778339862823]}
{"scores":[0.280087798833847,0.368331134319305,0.351581096649169]}
```

In both the classification and regression formats, the scores apply to individual labels\. 