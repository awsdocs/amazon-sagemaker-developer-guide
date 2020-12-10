# Difference in proportions of labels \(DPL\)<a name="clarify-data-bias-metric-true-label-imbalance"></a>

The difference in proportions of labels \(DPL\) compares the proportion of observed outcomes with positive labels for a disadvantaged facet *d* with the prpportion of observed outcomes with positive labels of an advantaged facet *a* in a training data set\. For example, it could be used to compare the proportion of men \(facet *a*\) and women \(facet *d*\) approved for college admission or financial loans\. Machine learning models try to mimic the training data decisions as closely as possible\. So a machine learning model trained on a dataset with a high DPL is likely to reflect the same imbalance in its future predictions\.

The formula for the difference in proportions of labels:

        DPL = \(qa \- qd\)

where:
+ qa = na\(1\)/na is the proportion of the advantaged facet *a* who have an observed label value of 1\. For example, the proportion of men who get accepted to college\. Here na\(1\) represents the number of members of the advantaged facet *a* who get a positive outcome and na the is number of members of the advantaged facet\. 
+ qd = nd\(1\)/nd is the proportion of the disadvantaged facet *a* who have an observed label value of 1\. For example, women who get accepted to college\. Here nd\(1\) represents the number of members of the disadvantaged facet *d* who get a positive outcome and nd the is number of members of the disadvantaged facet\. 

If DPL is close enough to 0, then we say that “demographic parity” has been achieved\.

For binary and multi\-category facet labels, the normalized DPL values range over the interval \(\-1, 1\)\. For continuous labels, the values vary over the interval \[\-∞, \+∞\]\. 
+ Positive DPL values indicate that the advantaged facet is has a higher proportion of positive outcomes when compared with the disadvantaged facet\.
+ Negative DPL values indicate that the disadvantaged facet is has a higher proportion of positive outcomes when compared with the advantaged facet\.
+ Values of DPL near zero indicate a more equal proportion of positive outcomes between facets and a value of zero indicates perfect demographic parity\. 

Whether or not a high magnitude of DPL is problematic varies from one situation to another\. In a problematic case, a high\-magnitude DPL might be a signal of underlying issues in the data\. For example, a dataset with high DPL might reflect historical biases or prejudices against gender or racial groups that would be undesirable for a model to learn\.