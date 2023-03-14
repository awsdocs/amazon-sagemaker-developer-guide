# Treatment Equality \(TE\)<a name="clarify-post-training-bias-metric-te"></a>

The treatment equality \(TE\) is the difference in the ratio of false negatives to false positives between facets *a* and *d*\. The main idea of this metric is to assess whether, even if the accuracy across groups is the same, is it the case that errors are more harmful to one group than another? Error rate comes from the total of false positives and false negatives, but the breakdown of these two maybe very different across facets\. TE measures whether errors are compensating in the similar or different ways across facets\. 

The formula for the treatment equality:

        TE = FNd/FPd \- FNa/FPa

Where:
+ FNd are the false negatives predicted for facet *d*\.
+ FPd are the false positives predicted for facet *d*\.
+ FNa are the false negatives predicted for facet *a*\.
+ FPa are the false positives predicted for facet *a*\.

Note the metric becomes unbounded if FPa or FPd is zero\.

For example, suppose that there are 100 loan applicants from facet *a* and 50 from facet *d*\. For facet *a*, 8 were wrongly denied a loan \(FNa\) and another 6 were wrongly approved \(FPa\)\. The remaining predictions were true, so TPa \+ TNa = 86\. For facet *d*, 5 were wrongly denied \(FNd\) and 2 were wrongly approved \(FPd\)\. The remaining predictions were true, so TPd \+ TNd = 43\. The ratio of false negatives to false positives equals 8/6 = 1\.33 for facet *a* and 5/2 = 2\.5 for facet *d*\. Hence TE = 2\.5 \- 1\.33 = 1\.167, even though both facets have the same accuracy:

        ACCa = \(86\)/\(86\+ 8 \+ 6\) = 0\.86

        ACCd = \(43\)/\(43 \+ 5 \+ 2\) = 0\.86

The range of values for differences in conditional rejection for binary and multicategory facet labels is \(\-∞, \+∞\)\. The TE metric is not defined for continuous labels\. The interpretation of this metric depends on the relative important of false positives \(Type I error\) and false negatives \(Type II error\)\. 
+ Positive values occur when the ratio of false negatives to false positives for facet *d* is greater than that for facet *a*\. 
+ Values near zero occur when the ratio of false negatives to false positives for facet *a* is similar to that for facet *d*\. 
+ Negative values occur when the ratio of false negatives to false positives for facet *d* is less than that for facet *a*\.

**Note**  
A previous version stated that the Treatment Equality metric is computed as FPa / FNa \- FPd / FNd instead of FNd / FPd \- FNa / FPa\. While either of the versions can be used\. For more information, see [https://pages.awscloud.com/rs/112-TZM-766/images/Fairness.Measures.for.Machine.Learning.in.Finance.pdf](https://pages.awscloud.com/rs/112-TZM-766/images/Fairness.Measures.for.Machine.Learning.in.Finance.pdf)\.