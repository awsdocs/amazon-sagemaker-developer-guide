# Difference in Rejection Rates \(DRR\)<a name="clarify-post-training-bias-metric-drr"></a>

The difference in rejection rates \(DRR\) metric is the difference in the ratios of the true negative \(TN\) predictions to the observed negatives \(TN \+ FN\) for facets *a* and *d*\. This metric measures the difference in the precision of the model for predicting rejections from these two facets\. Precision measures the fraction of unqualified candidates from the pool of unqualified candidates that are identified as such by the model\. If the model precision for predicting unqualified applicants diverges between the facets, this is a bias and its magnitude is measured by the DRR\.

The formula for difference in rejection rates between facets *a* and *d*:

        DRR = TNd/\(TNd \+ FNd\) \- TNa/\(TNa \+ FNa\) 

Where:
+ TNd are the true negatives predicted for facet *d*\.
+ FNd are the false negatives predicted for facet *d*\.
+ TPa are the true negatives predicted for facet *a*\.
+ FNa are the false negatives predicted for facet *a*\.

For example, suppose the model rejects 100 middle\-aged applicants \(facet *a*\) for a loan \(predicted negative labels\) of whom 80 are actually unqualified \(observed negative labels\)\. Also suppose the model rejects 50 applicants from other age demographics \(facet *d*\) for a loan \(predicted negative labels\) of whom only 40 are actually unqualified \(observed negative labels\)\. Then DRR = 40/50 \- 80/100 = 0, so no bias is indicated\.

The range of values for DRR for binary, multicategory facet, and continuous labels is \[\-1, \+1\]\.
+ Positive values occur when the ratio of the predicted negatives \(rejections\) to the observed negative outcomes \(unqualified applicants\) for facet *d* is larger than the same ratio for facet *a*\. These values indicate a possible bias against the favored facet *a* caused by the occurrence of relatively more false negatives in facet *a*\. The larger the difference in the ratios, the more extreme the apparent bias\.
+ Values near zero occur when the ratio of the predicted negatives \(rejections\) to the observed negative outcomes \(unqualified applicants\) for facets *a* and *d* have similar values, indicating the observed labels for negative outcomes are being predicted with equal precision by the model\.
+ Negative values occur when the ratio of the predicted negatives \(rejections\) to the observed negative outcomes \(unqualified applicants\) for facet *a* is larger than the ratio facet *d*\. These values indicate a possible bias against the disfavored facet *d* caused by the occurrence of relatively more false positives in facet *d*\. The more negative the difference in the ratios, the more extreme the apparent bias\.