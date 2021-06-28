# Conditional Demographic Disparity in Predicted Labels \(CDDPL\)<a name="clarify-post-training-bias-metric-cddpl"></a>

The demographic disparity metric \(DDPL\) determines whether facet *d* has a larger proportion of the predicted rejected labels than of the predicted accepted labels\. It enables a comparison of difference in predicted rejection proportion and predicted acceptance proportion across facets\. This metric is exactly the same as the pretraining CDD metric except that it is computed off the predicted labels instead of the observed ones\. This metric lies in the range \(\-1,\+1\)\.

The formula for the demographic disparity predictions for labels of facet *d* is as follows: 

 DDPLd = n'd\(0\)/n'\(0\) \- n'd\(1\)/n'\(1\) = PdR\(y'0\) \- PdA\(y'1\) 

Where: 
+ n'\(0\) = n'a\(0\) \+ n'd\(0\) is the number of predicted rejected labels for facets *a* and *d*\.
+ n'\(1\) = n'a\(1\) \+ n'd\(1\) is the number of predicted accepted labels for facets *a* and *d*\.
+ PdR\(y'0\) is the proportion of predicted rejected labels \(value 0\) in facet *d*\.
+ PdA\(y'1\) is the proportion of predicted accepted labels \(value 1\) in facet *d*\.

A conditional demographic disparity in predicted labels \(CDDPL\) metric that conditions DDPL on attributes that define a strata of subgroups on the dataset is needed to rule out Simpson's paradox\. The regrouping can provide insights into the cause of apparent demographic disparities for less favored facets\. The classic case arose in the case of Berkeley admissions where men were accepted at a higher rate overall than women\. But when departmental subgroups were examined, women were shown to have higher admission rates than men by department\. The explanation was that women had applied to departments with lower acceptance rates than men had\. Examining the subgroup acceptance rates revealed that women were actually accepted at a higher rate than men for the departments with lower acceptance rates\.

The CDDPL metric gives a single measure for all of the disparities found in the subgroups defined by an attribute of a dataset by averaging them\. It is defined as the weighted average of demographic disparities in predicted labels \(DDPLi\) for each of the subgroups, with each subgroup disparity weighted in proportion to the number of observations in contains\. The formula for the conditional demographic disparity in predicted labels is as follows:

 CDDPL = \(1/n\)\*∑ini \*DDPLi 

Where: 
+ ∑ini = n is the total number of observations and niis the number of observations for each subgroup\.
+ DDPLi = n'i\(0\)/n\(0\) \- n'i\(1\)/n\(1\) = PiR\(y'0\) \- PiA\(y'1\) is the demographic disparity in predicted labels for the subgroup\.

So the demographic disparity for a subgroup in predicted labels \(DDPLi\) are the difference between the proportion of predicted rejected labels and the proportion of predicted accepted labels for each subgroup\. 

The range of DDPL values for binary, multicategory, and continuous outcomes is \[\-1,\+1\]\. 
+ \+1: when there are no predicted rejection labels for facet *a* or subgroup and no predicted acceptances for facet *d* or subgroup\.
+ Positive values indicate there is a demographic disparity in predicted labels as facet *d* or subgroup has a larger proportion of the predicted rejected labels than of the predicted accepted labels\. The higher the value the greater the disparity\.
+ Values near zero indicate there is no demographic disparity on average\.
+ Negative values indicate there is a demographic disparity in predicted labels as facet *a* or subgroup has a larger proportion of the predicted rejected labels than of the predicted accepted labels\. The lower the value the greater the disparity\.
+ \-1: when there are no predicted rejection lapels for facet *d* or subgroup and no predicted acceptances for facet *a* or subgroup\.