# Conditional Demographic Disparity \(CDD\)<a name="clarify-data-bias-metric-cddl"></a>

The demographic disparity metric \(DD\) determines whether a facet has a larger proportion of the rejected outcomes in the dataset than of the accepted outcomes\. In the binary case where there are two facets, men and women for example, that constitute the dataset, the disfavored one is labelled facet *d* and the favored one is labelled facet *a*\. For example, in the case of college admissions, if women applicants comprised 46% of the rejected applicants and comprised only 32% of the accepted applicants, we say that there is *demographic disparity* because the rate at which women were rejected exceeds the rate at which they are accepted\. Women applicants are labelled facet *d* in this case\. If men applicants comprised 54% of the rejected applicants and 68% of the accepted applicants, then there is not a demographic disparity for this facet as the rate of rejection is less that the rate of acceptance\. Men applicants are labelled facet *a* in this case\. 

The formula for the demographic disparity for the less favored facet *d* is as follows: 

        DDd = nd\(0\)/n\(0\) \- nd\(1\)/n\(1\) = PdR\(y0\) \- PdA\(y1\) 

Where: 
+ n\(0\) = na\(0\) \+ nd\(0\) is the total number of rejected outcomes in the dataset for the favored facet *a* and disadvantaged facet *d*\.
+ n\(1\) = na\(1\) \+ nd\(1\) is the total number of accepted outcomes in the dataset for the favored facet *a* and disadvantaged facet *d*\.
+ PdR\(y0\) is the proportion of rejected outcomes \(with value 0\) in facet *d*\.
+ PdA\(y1\) is the proportion of accepted outcomes \(value 1\) in facet *d*\.

For the college admission example, the demographic disparity for women is DDd = 0\.46 \- 0\.32 = 0\.14\. For men DDa = 0\.54 \- 0\.68 = \- 0\.14\.

A conditional demographic disparity \(CDD\) metric that conditions DD on attributes that define a strata of subgroups on the dataset is needed to rule out Simpson's paradox\. The regrouping can provide insights into the cause of apparent demographic disparities for less favored facets\. The classic case arose in the case of Berkeley admissions where men were accepted at a higher rate overall than women\. The statistics for this case were used in the example calculations of DD\. However, when departmental subgroups were examined, women were shown to have higher admission rates than men when conditioned by department\. The explanation was that women had applied to departments with lower acceptance rates than men had\. Examining the subgrouped acceptance rates revealed that women were actually accepted at a higher rate than men for the departments with lower acceptance rates\.

The CDD metric gives a single measure for all of the disparities found in the subgroups defined by an attribute of a dataset by averaging them\. It is defined as the weighted average of demographic disparities \(DDi\) for each of the subgroups, with each subgroup disparity weighted in proportion to the number of observations in contains\. The formula for the conditional demographic disparity is as follows:

        CDD = \(1/n\)\*∑ini \*DDi 

Where: 
+ ∑ini = n is the total number of observations and niis the number of observations for each subgroup\.
+ DDi = ni\(0\)/n\(0\) \- ni\(1\)/n\(1\) = PiR\(y0\) \- PiA\(y1\) is the demographic disparity for the ith subgroup\.

The demographic disparity for a subgroup \(DDi\) are the difference between the proportion of rejected outcomes and the proportion of accepted outcomes for each subgroup\.

The range of DD values for binary outcomes for the full dataset DDd or for its conditionalized subgroups DDi is \[\-1, \+1\]\. 
+ \+1: when there no rejections in facet *a* or subgroup and no acceptances in facet *d* or subgroup
+ Positive values indicate there is a demographic disparity as facet *d* or subgroup has a greater proportion of the rejected outcomes in the dataset than of the accepted outcomes\. The higher the value the less favored the facet and the greater the disparity\.
+ Negative values indicate there is not a demographic disparity as facet *d* or subgroup has a larger proportion of the accepted outcomes in the dataset than of the rejected outcomes\. The lower the value the more favored the facet\.
+ \-1: when there are no rejections in facet *d* or subgroup and no acceptances in facet *a* or subgroup

If you don't condition on anything then CDD is zero if and only if DPL is zero\.

This metric is useful for exploring the concepts of direct and indirect discrimination and of objective justification in EU and UK non\-discrimination law and jurisprudence\. For additional information, see [Why Fairness Cannot Be Automated](https://arxiv.org/abs/2005.05906)\. This paper also contains the relevant data and analysis of the Berkeley admissions case that shows how conditionalizing on departmental admission rate subgroups illustrates Simpson's paradox\.