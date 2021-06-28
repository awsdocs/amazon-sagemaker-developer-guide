# Disparate Impact \(DI\)<a name="clarify-post-training-bias-metric-di"></a>

The difference in positive proportions in the predicted labels metric can be assessed in the form of a ratio\.

The comparison of positive proportions in predicted labels metric can be assessed in the form of a ratio instead of as a difference, as it is with the [Difference in Positive Proportions in Predicted Labels \(DPPL\)](clarify-post-training-bias-metric-dppl.md)\. The disparate impact \(DI\) metric is defined as the ratio of the proportion of positive predictions \(y’ = 1\) for facet *a* over the proportion of positive predictions \(y’ = 1\) for facet *d*\. For example, if the model predictions grant loans to 60% of a middle\-aged group \(facet *a*\) and 50% other age groups \(facet *d*\), then DI = \.5/\.6 = 0\.8, which indicates a positive bias and an adverse impact on facet *d*\.

The formula for the ratio of proportions of the predicted labels:



 DI = q'd/q'a

Where:
+ q'a = n'a\(1\)/na is the predicted proportion of facet *a* who get a positive outcome of value 1\. In our example, the proportion of a middle\-aged facet predicted to get granted a loan\. Here n'a\(1\) represents the number of members of facet *a* who get a positive predicted outcome and na the is number of members of facet *a*\. 
+ q'd = n'd\(1\)/nd is the predicted proportion of facet *d* a who get a positive outcome of value 1\. In our example, a facet of older and younger people predicted to get granted a loan\. Here n'd\(1\) represents the number of members of facet *d* who get a positive predicted outcome and nd the is number of members of facet *d*\. 

For binary, multicategory facet, and continuous labels, the DI values range over the interval \[0, ∞\)\.
+ Values less than 1 indicate that facet *a* has a higher proportion of predicted positive outcomes than facet *d*\. This is referred to as *positive bias*\.
+ A value of 1 indicates demographic parity\. 
+ Values greater than 1 indicate that facet *d* has a higher proportion of predicted positive outcomes than facet *a*\. This is referred to as *negative bias*\.