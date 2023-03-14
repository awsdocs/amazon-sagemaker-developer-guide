# Specificity difference \(SD\)<a name="clarify-post-training-bias-metric-sd"></a>

The specificity difference \(SD\) is the difference in specificity between the favored facet *a* and disfavored facet *d*\. Specificity measures how often the model correctly predicts a negative outcome \(y'=0\)\. Any difference in these specificities is a potential form of bias\. 

Specificity is perfect for a facet if all of the y=0 cases are correctly predicted for that facet\. Specificity is greater when the model minimizes false positives, known as a Type I error\. For example, the difference between a low specificity for lending to facet *a*, and high specificity for lending to facet *d*, is a measure of bias against facet *d*\.

The following formula is for the difference in the specificity for facets *a* and *d*\.

        SD = TNd/\(TNd \+ FPd\) \- TNa/\(TNa \+ FPa\) = TNRd \- TNRa

The following variables used to calculated SD are defined as follows:
+ TNd are the true negatives predicted for facet *d*\.
+ FPd are the false positives predicted for facet *d*\.
+ TNd are the true negatives predicted for facet *a*\.
+ FPd are the false positives predicted for facet *a*\.
+ TNRa = TNa/\(TNa \+ FPa\) is the true negative rate, also known as the specificity, for facet *a*\.
+ TNRd = TNd/\(TNd \+ FPd\) is the true negative rate, also known as the specificity, for facet *d*\.

For example, consider the following confusion matrices for facets *a* and *d*\.


**Confusion matrix for the favored facet `a`**  

| Class a predictions | Actual outcome 0 | Actual outcome 1 | Total  | 
| --- | --- | --- | --- | 
| 0 | 20 | 5 | 25 | 
| 1 | 10 | 65 | 75 | 
| Total | 30 | 70 | 100 | 


**Confusion matrix for the disfavored facet `d`**  

| Class d predictions | Actual outcome 0 | Actual outcome 1 | Total  | 
| --- | --- | --- | --- | 
| 0 | 18 | 7 | 25 | 
| 1 | 5 | 20 | 25 | 
| Total | 23 | 27 | 50 | 

The value of the specificity difference is `SD = 18/(18+5) - 20/(20+10) = 0.7826 - 0.6667 = 0.1159`, which indicates a bias against facet *d*\.

The range of values for the specificity difference between facets *a* and *d* for binary and multicategory classification is `[-1, +1]`\. This metric is not available for the case of continuous labels\. Here is what different values of SD imply:
+ Positive values are obtained when there is higher specificity for facet *d* than for facet *a*\. This suggests that the model finds less false positives for facet *d* than for facet *a*\. A positive value indicates bias against facet *d*\. 
+ Values near zero indicate that the specificity for facets that are being compared is similar\. This suggests that the model finds a similar number of false positives in both of these facets and is not biased\.
+ Negative values are obtained when there is higher specificity for facet *a* than for facet *d*\. This suggests that the model finds more false positives for facet *a* than for facet *d*\. A negative value indicates bias against facet *a*\. 