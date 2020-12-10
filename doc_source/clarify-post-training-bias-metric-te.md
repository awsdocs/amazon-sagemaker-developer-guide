# Treatment equality \(TE\)<a name="clarify-post-training-bias-metric-te"></a>

The treatment equality \(TE\) is the difference in the ratio of false positives to false negatives for the advantaged and disadvantaged facets\. The main idea of this metric is to assess whether, even if the accuracy across groups is the same, is it the case that errors are more harmful to one group than another? Error rate comes from the total of false positives and false negatives, but the breakdown of these two maybe very different across protected facets\. TE measures whether errors are compensating in the same way across facets\. 

The formula for the treatment equality:

        TE = FPa/FNa \- FPd/FNd

where:
+ FPa are the false positives predicted for the advantaged facet\.
+ FNa are the false negatives predicted for the advantaged facet\.
+ FPd are the false positives predicted for the disadvantaged facet\.
+ FNd are the false negatives predicted for the disadvantaged facet\.

Note the metric becomes unbounded if FNa or FNd is zero\.

For example, suppose that 100 men and 50 women apply for a loan\. For men, 8 were wrongly denied a loan \(FNa\) and another 6 were wrongly approved \(FPa\)\. The remaining predictions were true, so TPa \+ TNa = 86\. For women, 5 were wrongly denied \(FNd\) and 2 were wrongly approved \(FPd\)\. The remaining predictions were true, so TPd \+ TNd = 43\. The ratio of false positives to false negatives equals 6/8 = 0\.75 for men and 2/5 = 0\.40 for women\. Hence TE = 0\.75 \- 0\.40 = 0\.35, even though both facets have the same accuracy:

        ACCa = \(86\)/\(68\+ 8 \+ 6\) = 0\.86

        ACCd = \(43\)/\(43 \+ 5 \+ 2\) = 0\.86

The range of values for differences in conditional rejection for binary and multi\-category facet labels is \(\-∞, \+∞\)\. The TE metric is not defined for continuous labels\. The interpretation of this metric depends on the relative important of false positives \(Type I error\) and false negatives \(Type II error\)\. 
+ Positive values occur when the ratio of false positives to false negatives for the advantaged facet is more than that for the disadvantaged facet\. 
+ Values near zero occur when the ratio of false positives to false negatives for the advantaged facet is similar to that for the disadvantaged facet\. 
+ Negative values occur when the ratio of false positives to false negatives for the advantaged facet is less than that for the disadvantaged facet\. 