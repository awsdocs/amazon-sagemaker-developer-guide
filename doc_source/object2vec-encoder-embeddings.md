# Encoder Embeddings for Object2Vec<a name="object2vec-encoder-embeddings"></a>

## INPUT: ENCODER EMBEDDINGS<a name="object2vec-in-encoder-embeddings-data"></a>

Content\-type: application/json

```
{
  "instances" : [
    {"in0": [6, 17, 606, 19, 53, 67, 52, 12, 5, 10, 15, 10178, 7, 33, 652, 80, 15, 69, 821, 4]},
    {"in0": [22, 1016, 32, 13, 25, 11, 5, 64, 573, 45, 5, 80, 15, 67, 21, 7, 9, 107, 4]},
    {"in0": [774, 14, 21, 206]}
  ]
}
```

Content\-type: application/jsonlines

```
{"in0": [6, 17, 606, 19, 53, 67, 52, 12, 5, 10, 15, 10178, 7, 33, 652, 80, 15, 69, 821, 4]}
{"in0": [22, 1016, 32, 13, 25, 11, 5, 64, 573, 45, 5, 80, 15, 67, 21, 7, 9, 107, 4]}
{"in0": [774, 14, 21, 206]}
```

In both these formats, you specify only one input type, either `“in0”` or `“in1.”` The inference service then invokes the corresponding encoder and outputs the embeddings for each of the instances\. 

## OUTPUT: ENCODER EMBEDDINGS<a name="object2vec-out-encoder-embeddings-data"></a>

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

The vector length of the embeddings output by the inference service is equal to the value of one of the hyperparameters, which you specify at training time: `enc0_token_embedding_dim`, `enc1_token_embedding_dim`, or `enc_dim`\.