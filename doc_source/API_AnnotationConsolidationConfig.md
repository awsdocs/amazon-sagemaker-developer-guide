# AnnotationConsolidationConfig<a name="API_AnnotationConsolidationConfig"></a>

Configures how labels are consolidated across human workers\.

## Contents<a name="API_AnnotationConsolidationConfig_Contents"></a>

 **AnnotationConsolidationLambdaArn**   <a name="SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn"></a>
The Amazon Resource Name \(ARN\) of a Lambda function implements the logic for annotation consolidation\.  
For the built\-in bounding box, image classification, semantic segmentation, and text classification task types, Amazon SageMaker Ground Truth provides the following Lambda functions:  
+  *Bounding box* \- Finds the most similar boxes from different workers based on the Jaccard index of the boxes\.

   `arn:aws:lambda:us-east-1:432418664414:function:ACS-BoundingBox` 

   `arn:aws:lambda:us-east-2:266458841044:function:ACS-BoundingBox` 

   `arn:aws:lambda:us-west-2:081040173940:function:ACS-BoundingBox` 

   `arn:aws:lambda:eu-west-1:568282634449:function:ACS-BoundingBox` 

   `arn:aws:lambda:ap-northeast-1:477331159723:function:ACS-BoundingBox` 
+  *Image classification* \- Uses a variant of the Expectation Maximization approach to estimate the true class of an image based on annotations from individual workers\.

   `arn:aws:lambda:us-east-1:432418664414:function:ACS-ImageMultiClass` 

   `arn:aws:lambda:us-east-2:266458841044:function:ACS-ImageMultiClass` 

   `arn:aws:lambda:us-west-2:081040173940:function:ACS-ImageMultiClass` 

   `arn:aws:lambda:eu-west-1:568282634449:function:ACS-ImageMultiClass` 

   `arn:aws:lambda:ap-northeast-1:477331159723:function:ACS-ImageMultiClass` 
+  *Semantic segmentation* \- Treats each pixel in an image as a multi\-class classification and treats pixel annotations from workers as "votes" for the correct label\.

   `arn:aws:lambda:us-east-1:432418664414:function:ACS-SemanticSegmentation` 

   `arn:aws:lambda:us-east-2:266458841044:function:ACS-SemanticSegmentation` 

   `arn:aws:lambda:us-west-2:081040173940:function:ACS-SemanticSegmentation` 

   `arn:aws:lambda:eu-west-1:568282634449:function:ACS-SemanticSegmentation` 

   `arn:aws:lambda:ap-northeast-1:477331159723:function:ACS-SemanticSegmentation` 
+  *Text classification* \- Uses a variant of the Expectation Maximization approach to estimate the true class of text based on annotations from individual workers\.

   `arn:aws:lambda:us-east-1:432418664414:function:ACS-TextMultiClass` 

   `arn:aws:lambda:us-east-2:266458841044:function:ACS-TextMultiClass` 

   `arn:aws:lambda:us-west-2:081040173940:function:ACS-TextMultiClass` 

   `arn:aws:lambda:eu-west-1:568282634449:function:ACS-TextMultiClass` 

   `arn:aws:lambda:ap-northeast-1:477331159723:function:ACS-TextMultiClass` 
For more information, see [Annotation Consolidation](http://docs.aws.amazon.com/sagemaker/latest/dg/sms-annotation-consolidation.html)\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:lambda:[a-z]{2}-[a-z]+-\d{1}:\d{12}:function:[a-zA-Z0-9-_\.]+(:(\$LATEST|[a-zA-Z0-9-_]+))?`   
Required: Yes

## See Also<a name="API_AnnotationConsolidationConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/AnnotationConsolidationConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/AnnotationConsolidationConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/AnnotationConsolidationConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/AnnotationConsolidationConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/AnnotationConsolidationConfig) 