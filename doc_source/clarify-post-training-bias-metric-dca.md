# Difference in Conditional Acceptance \(DCAcc\)<a name="clarify-post-training-bias-metric-dca"></a>

This metric compares the observed labels to the labels predicted by the model and assesses whether this is the same across facets for predicted positive outcomes\. This metric comes close to mimicking human bias in that it quantifies how many more positive outcomes a model predicted \(labels y’\) for a certain facet as compared to what was observed in the training dataset \(labels y\)\. For example, if there were more acceptances \(a positive outcome\) for loan applications for a middle\-aged group \(facet *a*\) than predicted by the model based on qualifications as compared to the facet containing other age groups \(facet *d*\), this might indicate potential bias in the way loans were approved\.

The formula for the difference in conditional acceptance:

 DCAcc = ca \- cd

Where:
+ ca = na\(1\)/ n'a\(1\) is the ratio of the observed number of positive outcomes of value 1 \(acceptances\) of facet *a* to the predicted number of positive outcome \(acceptances\) for facet *a*\. 
+ cd = nd\(1\)/ n'd\(1\) is the ratio of the observed number of positive outcomes of value 1 \(acceptances\) of facet *d* to the predicted number of predicted positive outcomes \(acceptances\) for facet *d*\. 

The DCAcc metric can capture both positive and negative biases that reveal preferential treatment based on qualifications\. Consider the following instances of age\-based bias on loan acceptances\.

**Example 1: Positive bias** 

Suppose we have dataset of 100 middle\-aged people \(facet *a*\) and 50 people from other age groups \(facet *d*\) who applied for loans, where the model recommended that 60 from facet *a* and 30 from facet *d* be given loans\. So the predicted proportions are unbiased with respect to the DPPL metric, but the observed labels show that 70 from facet *a* and 20 from facet *d* were granted loans\. In other words, the model granted loans to 17% more from the middle aged facet than the observed labels in the training data suggested \(70/60 = 1\.17\) and granted loans to 33% less from other age groups than the observed labels suggested \(20/30 = 0\.67\)\. The calculation of the DCAcc value quantifies this difference between \+17% and \-33%\. The calculation of the DCAcc value gives the following:

 DCAcc = 70/60 \- 20/30 = 1/2

**Example 2: Negative bias** 

Suppose we have dataset of 100 middle\-aged people \(facet *a*\) and 50 people from other age groups \(facet *d*\) who applied for loans, where the model recommended that 60 from facet *a* and 30 from facet *d* be given loans\. So the predicted proportions are unbiased with respect to the DPPL metric, but the observed labels show that 50 from facet *a* and 40 from facet *d* were granted loans\. In other words, the model granted loans to 17% fewer from the middle aged facet than the observed labels in the training data suggested \(50/60 = 0\.83\), and granted loans to 33% more from other age groups than the observed labels suggested \(40/30 = 1\.33\)\. The calculation of the DCAcc value quantifies this difference between \-17% and \+33%\. The calculation of the DCAcc value gives the following:

 DCAcc = 50/60 \- 40/30 = \-1/2

Note that you can use DCAcc to help you detect potential \(unintentional\) biases by humans overseeing the model predictions in a human\-in\-the\-loop setting\. Assume, for example, that the predictions y' by the model were unbiased, but the eventual decision is made by a human \(possibly with access to additional features\) who can alter the model predictions to generate a new and final version of y'\. The additional processing by the human may unintentionally deny loans to a disproportionate number from one facet\. DCAcc can help detect such potential biases\.

The range of values for differences in conditional acceptance for binary, multicategory facet, and continuous labels is \(\-∞, \+∞\)\.
+ Positive values occur when the ratio of the observed number of acceptances compared to predicted acceptances for facet *a* is higher than the same ratio for facet *d*\. These values indicate a possible bias against the qualified applicants from facet *d*\. The larger the difference of the ratios, the more extreme the apparent bias\.
+ Values near zero occur when the ratio of the observed number of acceptances compared to predicted acceptances for facet *a* is the similar to the ratio for facet *d*\. These values indicate that predicted acceptance rates are consistent with the observed values in the labeled data and that qualified applicants from both facets are being accepted in a similar way\. 
+ Negative values occur when the ratio of the observed number of acceptances compared to predicted acceptances for facet *a* is less than that ratio for facet *d*\. These values indicate a possible bias against the qualified applicants from facet *a*\. The more negative the difference in the ratios, the more extreme the apparent bias\.