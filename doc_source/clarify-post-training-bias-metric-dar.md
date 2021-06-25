# Difference in Acceptance Rates \(DAR\)<a name="clarify-post-training-bias-metric-dar"></a>

The difference in acceptance rates \(DAR\) metric is the difference in the ratios of the true positive \(TP\) predictions to the observed positives \(TP \+ FP\) for facets *a* and *d*\. This metric measures the difference in the precision of the model for predicting acceptances from these two facets\. Precision measures the fraction of qualified candidates from the pool of qualified candidates that are identified as such by the model\. If the model precision for predicting qualified applicants diverges between the facets, this is a bias and its magnitude is measured by the DAR\.

The formula for difference in acceptance rates between facets *a* and *d*:

 DAR = TPa/\(TPa \+ FPa\) \- TPd/\(TPd \+ FPd\) 

Where:
+ TPa are the true positives predicted for facet *a*\.
+ FPa are the false positives predicted for facet *a*\.
+ TPd are the true positives predicted for facet *d*\.
+ FPd are the false positives predicted for facet *d*\.

For example, suppose the model accepts 70 middle\-aged applicants \(facet *a*\) for a loan \(predicted positive labels\) of whom only 35 are actually accepted \(observed positive labels\)\. Also suppose the model accepts 100 applicants from other age demographics \(facet *d*\) for a loan \(predicted positive labels\) of whom only 40 are actually accepted \(observed positive labels\)\. Then DAR = 35/70 \- 40/100 = 0\.10, which indicates a potential bias against qualified people from the second age group \(facet *d*\)\.

The range of values for DAR for binary, multicategory facet, and continuous labels is \[\-1, \+1\]\.
+ Positive values occur when the ratio of the predicted positives \(acceptances\) to the observed positive outcomes \(qualified applicants\) for facet *a* is larger than the same ratio for facet *d*\. These values indicate a possible bias against the disfavored facet *d* caused by the occurrence of relatively more false positives in facet *d*\. The larger the difference in the ratios, the more extreme the apparent bias\.
+ Values near zero occur when the ratio of the predicted positives \(acceptances\) to the observed positive outcomes \(qualified applicants\) for facets *a* and *d* have similar values indicating the observed labels for positive outcomes are being predicted with equal precision by the model\.
+ Negative values occur when the ratio of the predicted positives \(acceptances\) to the observed positive outcomes \(qualified applicants\) for facet *d* is larger than the ratio facet *a*\. These values indicate a possible bias against the favored facet *a* caused by the occurrence of relatively more false positives in facet *a*\. The more negative the difference in the ratios, the more extreme the apparent bias\.