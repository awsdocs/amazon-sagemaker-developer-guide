# Difference in conditional rejection \(DCR\)<a name="clarify-post-training-bias-metric-dcr"></a>

This metric compares the observed labels to the labels predicts by the model and assesses whether this is the same across facets for negative outcomes \(rejections\)\. This metric accounts for whether one group is deserving or not and then measures bias\. For example, if there were more rejections for loan applications for women than predicted by the model based on qualifications as compared to men, this may indicate potential bias in the way loans were approved\. 

The formula for the difference in conditional acceptance:

        DCR = rd \- ra

where:
+ rd = nd\(0\)/ n'd\(0\) is the ratio of the observed number of negative outcomes of value 1 \(rejections\) of the disadvantaged facet *a* to the predicted number of negative outcome \(rejections\) of the disadvantaged facet\. 
+ ra = na\(0\)/ n'a\(0\) is the ratio of the observed number of negative outcomes of value 0 \(rejections\) of the advantaged facet *a* to the predicted number of negative outcome of value 0 \(rejections\) of the advantaged facet\. 

The DCR metric can capture both positive and negative biases that reveal preferential treatment based on qualifications\. Consider the following instances of gender bias on loan rejections\.

Example 1: Positive bias\. Suppose we have data set of 100 men and 50 women who applied for loans where the model recommended that 60 men and 30 women be rejected for loans\. So the predicted proportions are fair by the DPPL metric, but the dataset shows that 50 men and 40 women were rejected instead, suggesting bias\. The calculation of the DCR value gives

        DCR = 40/30 \- 50/60 = 1/2

The lender is rejecting fewer loans to men and rejecting more loans to women than warranted by the model, suggesting bias favoring men against women\.

Example 2: Negative bias\. Suppose we have data set of 100 men and 50 women who applied for loans where the model recommended that 60 men and 30 women be rejected for loans\. So the predicted proportions are fair by the DPPL metric, but the dataset shows that 70 men and 20 women were rejected instead, suggesting bias\. The calculation of the DCR value gives

        DCR = 20/30 \- 70/60 = \-1/2

The lender is rejecting fewer loans to women and rejecting more loans to men than warranted by the model, suggesting bias favoring women against men\.

The range of values for differences in conditional rejection for binary, multi\-category facet, and continuous labels is \(\-∞, \+∞\)\.
+ Positive values occur when the ratio of the observed number of rejections compared to predicted rejections for the disadvantaged facet is greater than that ratio for the advantaged facet\. These values indicate a possible bias against the qualified applicants from the disadvantaged facet in the labeled data\. The larger the value of DCR metric, the more extreme the prima facie bias\.
+ Values near zero occur when the ratio of the observed number of rejections compared to predicted acceptances for the advantaged facet is the similar to the ratio for the disadvantaged facet\. These values indicate that predicted rejections rates are consistent with the observed values in the labeled data and that the qualified applicants from both advantaged and disadvantaged facets are being rejected in a DCR\-fair way\. 
+ Negative values occur when the ratio of the observed number of rejections compared to predicted rejections for the disadvantaged facet is less than that ratio for the advantaged facet\. These values indicate a possible bias against the qualified applicants from the advantaged facet in the labeled data\. The larger magnitude of the negative DCR metric, the more extreme the prima facie bias\.