# Difference in Positive Proportions in Predicted Labels \(DPPL\)<a name="clarify-post-training-bias-metric-dppl"></a>

The difference in positive proportions in predicted labels \(DPPL\) metric determines whether the model predicts outcomes differently for each facet\. It is defined as the difference between the proportion of positive predictions \(y’ = 1\) for facet *a* and the proportion of positive predictions \(y’ = 1\) for facet *d*\. For example, if the model predictions grant loans to 60% of a middle\-aged group \(facet *a*\) and 50% other age groups \(facet *d*\), it might be biased against facet *d*\. In this example, you need to determine whether the 10% difference is material to a case for bias\. A comparison of DPL with DPPL assesses whether bias initially present in the dataset increases or decreases in the model predictions after training\.

The formula for the difference in proportions of predicted labels:



        DPPL = q'a \- q'd

Where:
+ q'a = n'a\(1\)/na is the predicted proportion of facet *a* who get a positive outcome of value 1\. In our example, the proportion of a middle\-aged facet predicted to get granted a loan\. Here n'a\(1\) represents the number of members of facet *a* who get a positive predicted outcome of value 1 and na the is number of members of facet *a*\. 
+ q'd = n'd\(1\)/nd is the predicted proportion of facet *d* who get a positive outcome of value 1\. In our example, a facet of older and younger people predicted to get granted a loan\. Here n'd\(1\) represents the number of members of facet *d* who get a positive predicted outcome and nd the is number of members of facet *d*\. 

If DPPL is close enough to 0, it means that post\-training *demographic parity* has been achieved\.

For binary and multicategory facet labels, the normalized DPL values range over the interval \[\-1, 1\]\. For continuous labels, the values vary over the interval \(\-∞, \+∞\)\. 
+ Positive DPPL values indicate that facet *a* has a higher proportion of predicted positive outcomes when compared with facet *d*\. 

  This is referred to as *positive bias*\.
+ Values of DPPL near zero indicate a more equal proportion of predicted positive outcomes between facets *a* and *d* and a value of zero indicates perfect demographic parity\. 
+ Negative DPPL values indicate that facet *d* has a higher proportion of predicted positive outcomes when compared with facet *a*\. This is referred to as *negative bias*\.