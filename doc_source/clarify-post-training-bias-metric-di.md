# Disparate impact \(DI\)<a name="clarify-post-training-bias-metric-di"></a>

The difference in positive proportions in predicted labels metric may be assessed in the form of a ratio\.

The comparison of positive proportions in predicted labels metric may be assessed in the form of a ratio instead of as a difference, as it is with the [Difference in positive proportions in predicted labels \(DPPL\)](clarify-post-training-bias-metric-dppl.md) \. The disparate impact \(DI\) metric is defined as the ratio of the proportion of positive predictions \(y’ = 1\) for the advantaged facet over the proportion of positive predictions \(y’ = 1\) for the disadvantaged facet\. For example, if the model predictions grant loans to 50% of women and to 60% of men, then DI = \.6/\.5 = 1\.2 which indicates a positive bias and so an adverse impact on women\.

The formula for the ratio of proportions of the predicted labels:

        DI = q'd/q'a

where:
+ q'a = n'a\(1\)/na is the predicted proportion of the advantaged facet *a* who get a positive outcome of value 1\. For example, the proportion of men predicted to get accepted to college\. Here n'a\(1\) represents the number of members of the advantaged facet *a* who get a positive predicted outcome and na the is number of members of the advantaged facet\. 
+ q'd = n'd\(1\)/nd is the predicted proportion of the disadvantaged facet a who get a positive outcome of value 1\. For example, women predicted to get accepted to college\. Here n'd\(1\) represents the number of members of the disadvantaged facet *d* who get a positive predicted outcome and nd the is number of members of the disadvantaged facet\. 

For binary, multi\-category facet, and continuous labels, the DI values range over the interval \[0, ∞\)\.
+ Values less than 1 indicate the advantaged facet has a higher proportion of predicted positive outcomes\.
+ A value of 1 indicates that we have demographic parity\. 
+ Values greater than 1 indicate the disadvantaged facet has a higher proportion of predicted positive outcomes\. This is referred to as negative bias\.