# Difference in Proportions of Labels \(DPL\)<a name="clarify-data-bias-metric-true-label-imbalance"></a>

The difference in proportions of labels \(DPL\) compares the proportion of observed outcomes with positive labels for facet *d* with the proportion of observed outcomes with positive labels of facet *a* in a training dataset\. For example, you could use it to compare the proportion of middle\-aged individuals \(facet *a*\) and other age groups \(facet *d*\) approved for financial loans\. Machine learning models try to mimic the training data decisions as closely as possible\. So a machine learning model trained on a dataset with a high DPL is likely to reflect the same imbalance in its future predictions\.

The formula for the difference in proportions of labels is as follows:

 DPL = \(qa \- qd\)

Where:
+ qa = na\(1\)/na is the proportion of facet *a* who have an observed label value of 1\. For example, the proportion of a middle\-aged demographic who get approved for loans\. Here na\(1\) represents the number of members of facet *a* who get a positive outcome and na the is number of members of facet *a*\. 
+ qd = nd\(1\)/nd is the proportion of facet *d* who have an observed label value of 1\. For example, the proportion of people outside the middle\-aged demographic who get approved for loans\. Here nd\(1\) represents the number of members of the facet *d* who get a positive outcome and nd the is number of members of the facet *d*\. 

If DPL is close enough to 0, then we say that *demographic parity* has been achieved\.

For binary and multicategory facet labels, the DPL values range over the interval \(\-1, 1\)\. For continuous labels, we set a threshold to collapse the labels to binary\. 
+ Positive DPL values indicate that facet *a* is has a higher proportion of positive outcomes when compared with facet *d*\.
+ Values of DPL near zero indicate a more equal proportion of positive outcomes between facets and a value of zero indicates perfect demographic parity\. 
+ Negative DPL values indicate that facet *d* has a higher proportion of positive outcomes when compared with facet *a*\.

Whether or not a high magnitude of DPL is problematic varies from one situation to another\. In a problematic case, a high\-magnitude DPL might be a signal of underlying issues in the data\. For example, a dataset with high DPL might reflect historical biases or prejudices against age\-based demographic groups that would be undesirable for a model to learn\.