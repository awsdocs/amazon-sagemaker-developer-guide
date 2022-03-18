# Difference in Conditional Rejection \(DCR\)<a name="clarify-post-training-bias-metric-dcr"></a>

This metric compares the observed labels to the labels predicted by the model and assesses whether this is the same across facets for negative outcomes \(rejections\)\. This metric comes close to mimicking human bias, in that it quantifies how many more negative outcomes a model granted \(predicted labels y’\) to a certain facet as compared to what was suggested by the labels in the training dataset \(observed labels y\)\. For example, if there were more observed rejections \(a negative outcome\) for loan applications for a middle\-aged group \(facet *a*\) than predicted by the model based on qualifications as compared to the facet containing other age groups \(facet *d*\), this might indicate potential bias in the way loans were rejected that favored the middle\-aged group over other groups\.

The formula for the difference in conditional acceptance:

        DCR = rd \- ra

Where:
+ rd = nd\(0\)/ n'd\(0\) is the ratio of the observed number of negative outcomes of value 0 \(rejections\) of facet *d* to the predicted number of negative outcome \(rejections\) for facet *d*\. 
+ ra = na\(0\)/ n'a\(0\) is the ratio of the observed number of negative outcomes of value 0 \(rejections\) of facet *a* to the predicted number of negative outcome of value 0 \(rejections\) for facet *a*\. 

The DCR metric can capture both positive and negative biases that reveal preferential treatment based on qualifications\. Consider the following instances of age\-based bias on loan rejections\.

**Example 1: Positive bias** 

Suppose we have dataset of 100 middle\-aged people \(facet *a*\) and 50 people from other age groups \(facet *d*\) who applied for loans, where the model recommended that 60 from facet *a* and 30 from facet *d* be rejected for loans\. So the predicted proportions are unbiased by the DPPL metric, but the observed labels show that 50 from facet *a* and 40 from facet *d* were rejected\. In other words, the model rejected 17% more loans from the middle aged facet than the observed labels in the training data suggested \(50/60 = 0\.83\), and rejected 33% fewer loans from other age groups than the observed labels suggested \(40/30 = 1\.33\)\. The DCR value quantifies this difference in the ratio of observed to predicted rejection rates between the facets\. The positive value indicates that there is a potential bias favoring the middle aged group with lower rejection rates as compared with other groups than the observed data \(taken as unbiased\) indicate is the case\.

        DCR = 40/30 \- 50/60 = 1/2

**Example 2: Negative bias** 

Suppose we have dataset of 100 middle\-aged people \(facet *a*\) and 50 people from other age groups \(facet *d*\) who applied for loans, where the model recommended that 60 from facet *a* and 30 from facet *d* be rejected for loans\. So the predicted proportions are unbiased by the DPPL metric, but the observed labels show that 70 from facet *a* and 20 from facet *d* were rejected\. In other words, the model rejected 17% fewer loans from the middle aged facet than the observed labels in the training data suggested \(70/60 = 1\.17\), and rejected 33% more loans from other age groups than the observed labels suggested \(20/30 = 0\.67\)\. The negative value indicates that there is a potential bias favoring facet *a* with lower rejection rates as compared with the middle\-aged facet *a* than the observed data \(taken as unbiased\) indicate is the case\.

        DCR = 20/30 \- 70/60 = \-1/2

The range of values for differences in conditional rejection for binary, multicategory facet, and continuous labels is \(\-∞, \+∞\)\.
+ Positive values occur when the ratio of the observed number of rejections compared to predicted rejections for facet *d* is greater than that ratio for facet *a*\. These values indicate a possible bias against the qualified applicants from facet *a*\. The larger the value of DCR metric, the more extreme the apparent bias\.
+ Values near zero occur when the ratio of the observed number of rejections compared to predicted acceptances for facet *a* is the similar to the ratio for facet *d*\. These values indicate that predicted rejections rates are consistent with the observed values in the labeled data and that the qualified applicants from both facets are being rejected in a similar way\. 
+ Negative values occur when the ratio of the observed number of rejections compared to predicted rejections for facet *d* is less than that ratio facet *a*\. These values indicate a possible bias against the qualified applicants from facet *d*\. The larger magnitude of the negative DCR metric, the more extreme the apparent bias\.

 