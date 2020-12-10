# Difference in rejection rates \(DRR\)<a name="clarify-post-training-bias-metric-drr"></a>

The difference in rejection rates \(DRR\) metric is the difference in the ratios of the true negative outcomes \(TN\) to the predicted negatives \(TN \+ FN\) between the disadvantaged and the advantaged facets\. This metric measures whether the negative predictive value of the model is the same for applicants from advantaged and disadvantaged groups\. If they are not, then this is a bias whose magnitude is measured by the DRR\. Note that in the case of admission rates, the use of quotas can also cause differences in label rates, which would show up in this metric\. 

The formula for difference in rejection rates between the disadvantaged and advantaged facets:

        DRR = TNd/\(TNd \+ FNd\) \- TNa/\(TNa \+ FNa\) 

where:
+ TNd are the true negatives predicted for the disadvantaged facet\.
+ FNd are the false negatives predicted for the disadvantaged facet\.
+ TPa are the true negatives predicted for the advantaged facet\.
+ FNa are the false negatives predicted for the advantaged facet\.

For example, suppose a model designates 50 women as unqualified and rejects 40 of them and designates 100 men as unqualified and rejects 80 of them, then DRR = 40/50 \- 80/100 = 0, so no bias is indicated\. When both DAR and DRR are zero, it satisfies a condition known as “equalized odds\.”

The range of values for DRR for binary, multi\-category facet, and continuous labels is \[\-1, \+1\]\.
+ Positive values occur when the ratio of the true negative outcomes \(unqualified applicants\) to the predicted negatives for the disadvantaged facet is larger than the ratio for the advantaged facet\. These values indicate a possible bias caused by the occurrence of relatively more false negatives in the advantaged facet, reducing its rate of rejection\. The more negative the difference in the ratios, the more extreme the prima facie bias\.
+ Values near zero occur when the ratio of the true positive outcomes to the predicted positives for the advantaged and disadvantaged facets are similar\.
+ Negative values occur when the ratio of the true negative outcomes \(unqualified applicants\) to the predicted negatives for the advantaged facet is larger than the ratio for the disadvantaged facet\. These values indicate a possible bias caused by the occurrence of relatively more false negatives in the disadvantaged facet, reducing its rate of rejection\. The larger the difference in the ratios, the more extreme the prima facie bias\.