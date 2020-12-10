# Conditional demographic disparity in predicted labels \(CDDPL\)<a name="clarify-post-training-bias-metric-cddpl"></a>

The demographic disparity metric \(DDPL\) determines whether the disadvantaged facet has a larger proportion of the predicted rejection labels than of the predicted accept ion labels\. It enables a comparison of difference in predicted rejection proportion and predicted acceptance proportion across facets\. This metric is exactly the same as the pre\-training CDD metric except that it is computed off the predicted labels instead of the observed ones\. This metric lies in the range \(\-1,\+1\)\.

The formula for the demographic disparity predictions for labels of the disadvantaged facet d is : 

        DDPLd = n'd\(0\)/n'\(0\) \- n'd\(1\)/n'\(1\) = PdR\(y'0\) \- PdA\(y'1\) 

where 
+ n'\(0\) = n'a\(0\) \+ n'd\(0\) is the number of predicted rejected labels
+ n'\(1\) = n'a\(1\) \+ n'd\(1\) is the number of predicted accepted labels
+ PdR\(y'0\) is the proportion of predicted rejected labels \(value 0\) in the disadvantaged facet d\.
+ PdA\(y'1\) is the proportion of predicted accepted labels \(value 1\) in the disadvantaged facet d\.

A conditional demographic disparity in predicted labels \(CDDPL\) metric that conditions DDPL on attributes that define a strata of subgroups on the data set is needed to rule out Simpson's paradox\. The regrouping can provide insights into the cause of apparent demographic disparities for disadvantaged facets\. The classic case arose in the case of Berkeley admissions where men were accepted at a higher rate overall than women\. But when departmental subgroups were examined, women were shown to have higher admission rats than men by department\. The explanation was that women had applied to departments with lower acceptance rates than men had\. Examining the subgrouped acceptance rates revealed that women were actually accepted at a higher rate than men for the departments with lower acceptance rates\.

The CDDPL metric gives a single measure for all of the disparities found in the subgroups defined by an attribute of a data set by averaging them\. It is defined as the weighted average of demographic disparities in predicted labels \(DDPLi\) for each of the subgroups, with each subgroup disparity weighted in proportion to the number of observations in contains\. The formula for the conditional demographic disparity in predicted labels is 

        CDDPL = \(1/n\)\*∑ini \*DDPLi 

where 
+ ∑ini = n is the total number of observations and niis the number of observations for each subgroup
+ DDPLi = n'i\(0\)/n\(0\) \- n'i\(1\)/n\(1\) = PiR\(y'0\) \- PiA\(y'1\) is the demographic disparity in predicted labels for the ith subgroup\.

So the demographic disparity for a subgroup in predicted labels \(DDPLi\) are the difference between the proportion of predicted rejected labels and the proportion of predicted accepted labels for each subgroup\. 

The range of DDPL values for binary, multi\-category, and continuous outcomes is \[\-1,\+1\]\. 
+ \+1: when there are no predicted rejection labels for the advantaged facet or subgroup and no predicted acceptances for the disadvantaged facet or subgroup
+ \-1: when there are no predicted rejection lapels for the disadvantaged facet or subgroup and no predicted acceptances for the advantaged facet or subgroup
+ Positive values indicate there is a democratic disparity in predicted labels as the disadvantaged facet or subgroup has a larger proportion of the predicted rejected labels than of the predicted accepted labels\. The higher the value the greater the disparity\.
+ Negative values indicate there is a democratic disparity in predicted labels as the advantaged facet or subgroup has a larger proportion of the predicted rejected labels than of the predicted accepted labels\. The lower the value the greater the disparity\.