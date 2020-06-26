# NLPSequenceRatio Rule<a name="nlp-sequence-ratio"></a>

This rule calculates the ratio of specific tokens given the rest of the input sequence that is useful for optimizing performance\. For example, you can calculate the percentage of padding end\-of\-sentence \(EOS\) tokens in your input sequence\. If the number of EOS tokens is too high, an alternate bucketing strategy should be performed\. You also can calculate the percentage of unknown tokens in your input sequence\. If the number of unknown words is too high, an alternate vocabulary could be used\.

This rule is applicable to deep learning applications\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the NLPSequenceRatio Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `".*embedding0_input_0"` \(assuming an embedding as the initial layer of the network\)  | 
| token\_values |  A string of a list of the numerical values of the tokens\. For example, "3, 0"\. **Optional** Valid values: Comma\-separated string of numerical values Default value: `0`  | 
| token\_thresholds\_percent |  A string of a list of thresholds \(in percentages\) that correspond to each of the `token_values`\. For example,"50\.0, 50\.0"\. **Optional** Valid values: Comma\-separated string of floats Default value: `"50,50`  | 