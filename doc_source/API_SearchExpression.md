# SearchExpression<a name="API_SearchExpression"></a>

A multi\-expression that searches for the specified resource or resources in a search\. All resource objects that satisfy the expression's condition are included in the search results\. You must specify at least one subexpression, filter, or nested filter\. A `SearchExpression` can contain up to twenty elements\.

A `SearchExpression` contains the following components:
+ A list of `Filter` objects\. Each filter defines a simple Boolean expression comprised of a resource property name, Boolean operator, and value\.
+ A list of `NestedFilter` objects\. Each nested filter defines a list of Boolean expressions using a list of resource properties\. A nested filter is satisfied if a single object in the list satisfies all Boolean expressions\.
+ A list of `SearchExpression` objects\. A search expression object can be nested in a list of search expression objects\.
+ A Boolean operator: `And` or `Or`\.

## Contents<a name="API_SearchExpression_Contents"></a>

 **Filters**   <a name="SageMaker-Type-SearchExpression-Filters"></a>
A list of filter objects\.  
Type: Array of [Filter](API_Filter.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 20 items\.  
Required: No

 **NestedFilters**   <a name="SageMaker-Type-SearchExpression-NestedFilters"></a>
A list of nested filter objects\.  
Type: Array of [NestedFilters](API_NestedFilters.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 20 items\.  
Required: No

 **Operator**   <a name="SageMaker-Type-SearchExpression-Operator"></a>
A Boolean operator used to evaluate the search expression\. If you want every conditional statement in all lists to be satisfied for the entire search expression to be true, specify `And`\. If only a single conditional statement needs to be true for the entire search expression to be true, specify `Or`\. The default value is `And`\.  
Type: String  
Valid Values:` And | Or`   
Required: No

 **SubExpressions**   <a name="SageMaker-Type-SearchExpression-SubExpressions"></a>
A list of search expression objects\.  
Type: Array of [SearchExpression](#API_SearchExpression) objects  
Array Members: Minimum number of 1 item\. Maximum number of 20 items\.  
Required: No

## See Also<a name="API_SearchExpression_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/SearchExpression) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/SearchExpression) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/SearchExpression) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/SearchExpression) 