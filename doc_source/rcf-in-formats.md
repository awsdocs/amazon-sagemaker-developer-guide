# RCF Response Formats<a name="rcf-in-formats"></a>

All Amazon SageMaker built\-in algorithms adhere to the common input inference format described in [Common Data Formats \- Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\. Note that SageMaker Random Cut Forest supports both dense and sparse JSON and RecordIO formats\. This topic contains a list of the available output formats for the SageMaker RCF algorithm\.

## JSON Response Format<a name="RCF-json"></a>

ACCEPT: application/json\.

```
    {                                                                                                                                                                                                                                                                                    
        "scores":    [                                                                                                                                                                                                                                                                   
            {"score": 0.02},                                                                                                                                                                                                                                                             
            {"score": 0.25}                                                                                                                                                                                                                                                              
        ]                                                                                                                                                                                                                                                                                
    }
```

### JSONLINES Response Format<a name="RCF-jsonlines"></a>

ACCEPT: application/jsonlines\.

```
{"score": 0.02},
{"score": 0.25}
```

## RECORDIO Response Format<a name="rcf-recordio"></a>

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