# Treatment Equality \(TE\)<a name="clarify-post-training-bias-metric-te"></a>

The treatment equality \(TE\) is the difference in the ratio of false positives to false negatives between facets *a* and *d*\. The main idea of this metric is to assess whether, even if the accuracy across groups is the same, is it the case that errors are more harmful to one group than another? Error rate comes from the total of false positives and false negatives, but the breakdown of these two maybe very different across facets\. TE measures whether errors are compensating in the similar or different ways across facets\. 

The formula for the treatment equality:

        TE = FPa/FNa \- FPd/FNd

Where:
+ FPa are the false positives predicted for facet *a*\.
+ FNa are the false negatives predicted for facet *a*\.
+ FPd are the false positives predicted for facet *d*\.
+ FNd are the false negatives predicted for facet *d*\.

Note the metric becomes unbounded if FNa or FNd is zero\.

For example, suppose that there are 100 loan applicants from facet *a* and 50 from facet *d*\. For facet *a*, 8 were wrongly denied a loan \(FNa\) and another 6 were wrongly approved \(FPa\)\. The remaining predictions were true, so TPa \+ TNa = 86\. For facet *d*, 5 were wrongly denied \(FNd\) and 2 were wrongly approved \(FPd\)\. The remaining predictions were true, so TPd \+ TNd = 43\. The ratio of false positives to false negatives equals 6/8 = 0\.75 for facet *a* and 2/5 = 0\.40 for facet *d*\. Hence TE = 0\.75 \- 0\.40 = 0\.35, even though both facets have the same accuracy:

        ACCa = \(86\)/\(68\+ 8 \+ 6\) = 0\.86

        ACCd = \(43\)/\(43 \+ 5 \+ 2\) = 0\.86

The range of values for differences in conditional rejection for binary and multicategory facet labels is \(\-∞, \+∞\)\. The TE metric is not defined for continuous labels\. The interpretation of this metric depends on the relative important of false positives \(Type I error\) and false negatives \(Type II error\)\. 
+ Positive values occur when the ratio of false positives to false negatives for facet *a* is greater than that for facet *d*\. 
+ Values near zero occur when the ratio of false positives to false negatives for facet *a* is similar to that for facet *d*\. 
+ Negative values occur when the ratio of false positives to false negatives for facet *a* is less than that for facet *d*\. 