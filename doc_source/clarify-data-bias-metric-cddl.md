# Conditional Demographic Disparity \(CDD\)<a name="clarify-data-bias-metric-cddl"></a>

The demographic disparity metric \(DD\) determines whether facet d has a larger proportion of the rejected outcomes in the dataset than of the accepted outcomes\. For example, in the case of college admissions, if women applicants comprised 60% of the rejected applicants and comprised only 50% of the accepted applicants, we say that there is *demographic disparity* because the rate at which women were rejected exceeds the rate at which they are accepted\.

The formula for the demographic disparity for the less favored facet *d* is as follows: 

        DDd = nd\(0\)/n\(0\) \- nd\(1\)/n\(1\) = PdR\(y0\) \- PdA\(y1\) 

Where: 
+ n\(0\) = na\(0\) \+ nd\(0\) is the number of rejected outcomes in the dataset\.
+ n\(1\) = na\(1\) \+ nd\(1\) is the number of accepted outcomes in the dataset\.
+ PdR\(y0\) is the proportion of rejected outcomes \(with value 0\) in facet *d*\.
+ PdA\(y1\) is the proportion of accepted outcomes \(value 1\) in facet *d*\.

For the college admission example, the demographic disparity is DD = 0\.6 \- 0\.5 = 0\.1\. 

A conditional demographic disparity \(CDD\) metric that conditions DD on attributes that define a strata of subgroups on the dataset is needed to rule out Simpson's paradox\. The regrouping can provide insights into the cause of apparent demographic disparities for less favored facets\. The classic case arose in the case of Berkeley admissions where men were accepted at a higher rate overall than women\. However, when departmental subgroups were examined, women were shown to have higher admission rates than men by department\. The explanation was that women had applied to departments with lower acceptance rates than men had\. Examining the subgrouped acceptance rates revealed that women were actually accepted at a higher rate than men for the departments with lower acceptance rates\.

The CDD metric gives a single measure for all of the disparities found in the subgroups defined by an attribute of a dataset by averaging them\. It is defined as the weighted average of demographic disparities \(DDi\) for each of the subgroups, with each subgroup disparity weighted in proportion to the number of observations in contains\. The formula for the conditional demographic disparity is as follows:

        CDD = \(1/n\)\*∑ini \*DDi 

Where: 
+ ∑ini = n is the total number of observations and niis the number of observations for each subgroup\.
+ DDi = ni\(0\)/n\(0\) \- ni\(1\)/n\(1\) = PiR\(y0\) \- PiA\(y1\) is the demographic disparity for the ith subgroup\.

The demographic disparity for a subgroup \(DDi\) are the difference between the proportion of rejected outcomes and the proportion of accepted outcomes for each subgroup\. 

The range of DD values for binary outcomes is \(\-1, \+1\)\. 
+ \+1: when there are no rejections in facet *a* or subgroup and no acceptances in facet *d* or subgroup
+ Positive values indicate there is a demographic disparity as facet *d* or subgroup has a larger proportion of the rejected outcomes in the dataset than of the accepted outcomes\. The higher the value the greater the disparity\.
+ Negative values indicate there is a demographic disparity as facet *a* or subgroup has a smaller proportion of the rejected outcomes in the dataset than of the accepted outcomes\. The lower the value the greater the disparity\.
+ \-1: when there are no rejections in facet *d* or subgroup and no acceptances in facet *a* or subgroup

If you don't condition on anything then CDD is zero if and only if DPL is zero\.

This metric is useful for exploring the concepts of direct and indirect discrimination and of objective justification in EU and UK non\-discrimination law and jurisprudence\. For additional information, see [Why Fairness Cannot Be Automated](https://arxiv.org/abs/2005.05906)\. 
