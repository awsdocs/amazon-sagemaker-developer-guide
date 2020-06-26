# Encoder Embeddings for Object2Vec<a name="object2vec-encoder-embeddings"></a>

## GPU optimization: Encoder Embeddings<a name="object2vec-inference-gpu-optimize-encoder-embeddings"></a>

An embedding is a mapping from discrete objects, such as words, to vectors of real numbers\.

Due to GPU memory scarcity, the `INFERENCE_PREFERRED_MODE` environment variable can be specified to optimize on whether the [ Data Formats for Object2Vec Inference](object2vec-inference-formats.md) or the encoder embedding inference network is loaded into GPU\. If the majority of your inference is for encoder embeddings, specify `INFERENCE_PREFERRED_MODE=embedding`\. The following is a Batch Transform example of using 4 instances of p3\.2xlarge that optimizes for encoder embedding inference:

```
transformer = o2v.transformer(instance_count=4, 
                              instance_type="ml.p2.xlarge", 
                              max_concurrent_transforms=2,
                              max_payload=1,  # 1MB
                              strategy='MultiRecord',
                              env={'INFERENCE_PREFERRED_MODE': 'embedding'},  # only useful with GPU
                              output_path=output_s3_path)
```

## Input: Encoder Embeddings<a name="object2vec-in-encoder-embeddings-data"></a>

Content\-type: application/json; infer\_max\_seqlens=<FWD\-LENGTH>,<BCK\-LENGTH>

Where <FWD\-LENGTH> and <BCK\-LENGTH> are integers in the range \[1,5000\] and define the maximum sequence lengths for the forward and backward encoder\.

```
{
  "instances" : [
    {"in0": [6, 17, 606, 19, 53, 67, 52, 12, 5, 10, 15, 10178, 7, 33, 652, 80, 15, 69, 821, 4]},
    {"in0": [22, 1016, 32, 13, 25, 11, 5, 64, 573, 45, 5, 80, 15, 67, 21, 7, 9, 107, 4]},
    {"in0": [774, 14, 21, 206]}
  ]
}
```

Content\-type: application/jsonlines; infer\_max\_seqlens=<FWD\-LENGTH>,<BCK\-LENGTH>

Where <FWD\-LENGTH> and <BCK\-LENGTH> are integers in the range \[1,5000\] and define the maximum sequence lengths for the forward and backward encoder\.

```
{"in0": [6, 17, 606, 19, 53, 67, 52, 12, 5, 10, 15, 10178, 7, 33, 652, 80, 15, 69, 821, 4]}
{"in0": [22, 1016, 32, 13, 25, 11, 5, 64, 573, 45, 5, 80, 15, 67, 21, 7, 9, 107, 4]}
{"in0": [774, 14, 21, 206]}
```

In both of these formats, you specify only one input type: `“in0”` or `“in1.”` The inference service then invokes the corresponding encoder and outputs the embeddings for each of the instances\. 

## Output: Encoder Embeddings<a name="object2vec-out-encoder-embeddings-data"></a>

Content\-type: application/json

```
{
  "predictions": [
    {"embeddings":[0.057368703186511,0.030703511089086,0.099890425801277,0.063688032329082,0.026327300816774,0.003637571120634,0.021305780857801,0.004316598642617,0.0,0.003397724591195,0.0,0.000378780066967,0.0,0.0,0.0,0.007419463712722]},
    {"embeddings":[0.150190666317939,0.05145975202322,0.098204270005226,0.064249359071254,0.056249320507049,0.01513972133398,0.047553978860378,0.0,0.0,0.011533712036907,0.011472506448626,0.010696629062294,0.0,0.0,0.0,0.008508535102009]}
  ]
}
```

Content\-type: application/jsonlines

```
{"embeddings":[0.057368703186511,0.030703511089086,0.099890425801277,0.063688032329082,0.026327300816774,0.003637571120634,0.021305780857801,0.004316598642617,0.0,0.003397724591195,0.0,0.000378780066967,0.0,0.0,0.0,0.007419463712722]}
{"embeddings":[0.150190666317939,0.05145975202322,0.098204270005226,0.064249359071254,0.056249320507049,0.01513972133398,0.047553978860378,0.0,0.0,0.011533712036907,0.011472506448626,0.010696629062294,0.0,0.0,0.0,0.008508535102009]}
```

The vector length of the embeddings output by the inference service is equal to the value of one of the following hyperparameters that you specify at training time: `enc0_token_embedding_dim`, `enc1_token_embedding_dim`, or `enc_dim`\.