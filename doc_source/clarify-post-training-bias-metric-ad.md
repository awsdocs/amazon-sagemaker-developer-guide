# Accuracy difference \(AD\)<a name="clarify-post-training-bias-metric-ad"></a>

Accuracy difference \(AD\) metric is the difference between the prediction accuracy for the advantaged and disadvantaged facets\. This metric determines whether the classification by the model is more accurate for one facet than the other\. AD indicates whether one facet incurs a greater proportion of Type I and Type II errors\. But it cannot differentiate between Type I and Type II errors\. For example, the model may have equal accuracy for men and women, but the errors may be mostly false positives \(Type I errors\) for men and mostly false negatives \(Type II errors\) for women\. Also, if loan approvals are made with much higher accuracy for men than for women, either a greater proportion of qualified women are denied a loan \(FN\) or a greater proportion of unqualified women get a loan \(FP\) or both\. This can lead to within group unfairness for women even if the proportion of loans granted is nearly the same for both men and women, indicated by DPPL being close to zero\.

The formula for AD metric is the difference between the prediction accuracy for the advantaged facet, ACCa, minus that for the disadvantaged facet, ACCd:

        AD = ACCa \- ACCd

where:
+ ACCa = \(TPa \+ TNa\)/\(TPa \+ TNa \+ FPa \+ FNa\) 
  + TPa are the true positives predicted for the advantaged facet\.
  + TNa are the true negatives predicted for the advantaged facet\.
  + FPa are the false positives predicted for the advantaged facet\.
  + FNa are the false negatives predicted for the advantaged facet\.
+ ACCd = \(TPd \+ TNd\)/\(TPd \+ TNd \+ FPd \+ FNd\)
  + TPd are the true positives predicted for the disadvantaged facet\.
  + TNd are the true negatives predicted for the disadvantaged facet\.
  + FPd are the false positives predicted for the disadvantaged facet\.
  + FNd are the false negatives predicted for the disadvantaged facet\.

For example, suppose a model approves loans to 70 applicants from an advantaged group *a* of 100 and rejected the other 30\. 10 should not have been offered the loan \(FPa\) and 60 were approved that should have been \(TPa\)\. 20 of the rejections should have been approved \(FNa\) and 10 were correctly rejected \(TNa\)\. The accuracy for facet *a* is then

        ACCa = \(60 \+ 10\)/\(60 \+ 10 \+ 20 \+ 10\) = 0\.7

Next suppose a model approves loans to 50 applicants from a disadvantaged group *d* of 100 and rejected the other 50\. 10 should not have been offered the loan \(FPa\) and 40 were approved that should have been \(TPa\)\. 40 of the rejections should have been approved \(FNa\) and 10 were correctly rejected \(TNa\)\. The accuracy for facet *a* is then

        ACCd= \(40 \+ 10\)/\(40 \+ 10 \+ 40 \+ 10\) = 0\.5

The accuracy difference is thus AD = ACCa \- ACCd = 0\.7 \- 0\.5 = 0\.2\. This indicates there is a bias against the disadvantaged facet as the metric is positive\.

The range of values for AD for binary and multi\-category facet labels is \[\-1, \+1\]\.
+ Positive values occur when the prediction accuracy for the advantaged is greater than that for the disadvantaged facet\. It means that the disadvantaged facet suffers more from some combination of false positives \(Type I errors\) or false negatives \(Type II errors\)\. This means the is a bias against the disadvantaged facet\.
+ Values near zero occur when the prediction accuracy for the advantaged is similar to that for the disadvantaged facet\.
+ Negative values occur when the prediction accuracy for the disadvantaged is greater than that for the advantaged facet\. It means that the advantaged facet suffers more from some combination of false positives \(Type I errors\) or false negatives \(Type II errors\)\. This means the is a bias against the advantaged facet\.