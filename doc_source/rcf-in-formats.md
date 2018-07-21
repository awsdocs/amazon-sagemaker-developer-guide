# RCF Response Formats<a name="rcf-in-formats"></a>

All Amazon SageMaker built\-in algorithms adhere to the common input inference format described in [Common Data Formats \- Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\. Note that Amazon SageMaker Random Cut Forest supports both dense and sparse JSON and RecordIO formats\. Below, is a list the available output formats\.

## JSON<a name="RCF-json"></a>

ACCEPT: application/json\.

```
    {                                                                                                                                                                                                                                                                                    
        "scores":    [                                                                                                                                                                                                                                                                   
            {"score": 0.02},                                                                                                                                                                                                                                                             
            {"score": 0.25}                                                                                                                                                                                                                                                              
        ]                                                                                                                                                                                                                                                                                
    }
```

### JSONLINES<a name="RCF-jsonlines"></a>

ACCEPT: application/jsonlines\.

```
{"score": 0.02},
{"score": 0.25}
```

## RECORDIO<a name="rcf-recordio"></a>

ACCEPT: application/x\-recordio\-protobuf\.

```
    [                                                                                                                                                                                                                                                                                    
         Record = {                                                                                                                                                                                                                                                                           
             features = {},                                                                                                                                                                                                                                                                   
             label = {                                                                                                                                                                                                                                                                       
                 'score': {                                                                                                                                                                                                                                                                   
                     keys: [],                                                                                                                                                                                                                                                                
                     values: [0.25]  # float32                                                                                                                                                                                                                                                
                 }                                                                                                                                                                                                                                                                            
             }                                                                                                                                                                                                                                                                                
         },                                                                                                                                                                                                                                                                                   
         Record = {                                                                                                                                                                                                                                                                           
             features = {},                                                                                                                                                                                                                                                                   
             label = {                                                                                                                                                                                                                                                                       
                 'score': {                                                                                                                                                                                                                                                                   
                     keys: [],                                                                                                                                                                                                                                                                
                     values: [0.23]  # float32                                                                                                                                                                                                                                                
                 }                                                                                                                                                                                                                                                                            
             }                                                                                                                                                                                                                                                                                
         }                                                                                                                                                                                                                                                                                    
    ]
```