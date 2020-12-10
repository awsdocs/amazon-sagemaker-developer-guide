# Difference in positive proportions in predicted labels \(DPPL\)<a name="clarify-post-training-bias-metric-dppl"></a>

The difference in positive proportions in predicted labels \(DPPL\) metric determines whether the model predicts outcomes differently for each facet\. It is defined as the difference in the proportion of positive predictions \(y’ = 1\) for the advantaged facet and the proportion of positive predictions \(y’ = 1\) for the disadvantaged facet\. For example, if the model predictions grant loans to 50% of women and to 60% of men, then it may be biased against women\. We would have to understand whether a 10% difference is material\. A comparison of DPL with DPPL assesses whether bias initially present in the dataset increases or decreases in the model predictions after training\.

The formula for the \(normalized\) difference in proportions of predicted labels:

        DPPL = q'a \- q'd

where:
+ q'a = n'a\(1\)/na is the predicted proportion of the advantaged facet *a* who get a positive outcome of value 1\. For example, the proportion of men predicted to get accepted to college\. Here n'a\(1\) represents the number of members of the advantaged facet *a* who get a positive predicted outcome of value 1 and na the is number of members of the advantaged facet\. 
+ q'd = n'd\(1\)/nd is the predicted proportion of the disadvantaged facet *d* who get a positive outcome of value 1\. For example, women predicted to get accepted to college\. Here n'd\(1\) represents the number of members of the disadvantaged facet *d* who get a positive predicted outcome and nd the is number of members of the disadvantaged facet\. 

If DPPL is close enough to 0, then we say that post\-training “demographic parity” has been achieved\.

For binary and multi\-category facet labels, the normalized DPL values range over the interval \(\-1, 1\)\. For continuous labels, the values vary over the interval \[\-∞, \+∞\]\. 
+ Positive DPPL values indicate that the advantaged facet is has a higher proportion of predicted positive outcomes when compared with the disadvantaged facet\.
+ Negative DPPL values indicate that the disadvantaged facet is has a higher proportion of predicted positive outcomes when compared with the advantaged facet\. This is referred to as negative bias\.
+ Values of DPPL near zero indicate a more equal proportion of predicted positive outcomes between facets and a value of zero indicates perfect demographic parity\. 