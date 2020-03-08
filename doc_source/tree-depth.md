# TreeDepth Rule<a name="tree-depth"></a>

This rule measures the depth of trees in an XGBoost model\. XGBoost rejects splits if they don't improve loss\. This regularizes the training\. As a result, the tree might not grow as deep as defined in `max_depth`\.

This rule is valid only for the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the TreeDepth Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| depth |  The depth of the tree\. The depth of the tree is obtained by computing the base 2 logarithm of the largest node ID\. **Optional** Valid values: Float Default value: `4`  | 