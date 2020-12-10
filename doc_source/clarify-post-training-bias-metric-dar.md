# Difference in acceptance rates \(DAR\)<a name="clarify-post-training-bias-metric-dar"></a>

The difference in acceptance rates \(DAR\) metric is the difference in the ratios of the true positive outcomes \(TP\) to the predicted positives \(TP \+ FP\) between the advantaged and the disadvantaged facets\. This metric measures whether the precision of the model is the same for applicants from advantaged and disadvantaged facets\. Precision measures the fraction of qualified candidates, according to the observed labels, from the pool of candidates that are deemed qualified by the model\. If they are not, then this is a bias whose magnitude is measured by the DAR\.

The formula for difference in acceptance rates between the advantaged and disadvantaged facets:

        DAR = TPa/\(TPa \+ FPa\) \- TPd/\(TPd \+ FPd\) 

where:
+ TPa are the true positives predicted for the advantaged facet\.
+ FPa are the false positives predicted for the advantaged facet\.
+ TPd are the true positives predicted for the disadvantaged facet\.
+ FPd are the false positives predicted for the disadvantaged facet\.

For example, suppose the total number of women who apply to college is 150 and men who apply is 180\. If the model predicts that 100 women are qualified to be admitted \(predicted positive label\) of whom the college admits 40 \(observed positive label\) and 70 men are qualified to be admitted of whom the college admits 35, then DAR = 35/70 \- 40/100 = 0\.10, which indicates a bias against qualified women\.

The range of values for DAR for binary, multi\-category facet, and continuous labels is \[\-1, \+1\]\.
+ Positive values occur when the ratio of the true positive outcomes \(qualified applicants\) to the predicted positives for the advantaged facet is larger than the ratio for the disadvantaged facet\. These values indicate a possible bias against the disadvantaged facet caused by the occurrence of relatively more false positives in the disadvantaged facet, reducing its rate of rejection\. The larger the difference in the ratios, the more extreme the prima facie bias\.
+ Values near zero occur when the ratio of the true positive outcomes to the predicted positives for the advantaged and disadvantaged facets are similar\. DAR is related to the *equality of opportunity* metric\.
+ Negative values occur when the ratio of the true positive outcomes \(qualified applicants\) to the predicted positives for the disadvantaged facet is larger than the ratio for the advantaged facet\. These values indicate a possible bias against the advantaged facet caused by the occurrence of relatively more false positives in the advantaged facet, reducing its rate of rejection\. The more negative the difference in the ratios, the more extreme the prima facie bias\.