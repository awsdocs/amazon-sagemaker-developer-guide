# Kolmogorov\-Smirnov \(KS\)<a name="clarify-data-bias-metric-kolmogorov-smirnov"></a>

The Kolmogorov\-Smirnov bias metric \(KS\) is equal to the maximum divergence between labels in the distributions for facets *a* and *d* of a dataset\. The two\-sample KS test implemented by SageMaker Clarify complements the other measures of label imbalance by finding the most imbalanced label\. 

The formula for the Kolmogorov\-Smirnov metric is as follows: 

 KS = max\(\|Pa\(y\) \- Pd\(y\)\|\)

For example, assume a group of applicants \(facet *a*\) to college are rejected, waitlisted, or accepted at 40%, 40%, 20% respectively and that these rates for other applicants \(facet *d*\) are 20%, 10%, 70%\. Then the Kolmogorov\-Smirnov bias metric value is as follows:

KS = max\(\|0\.4\-0\.2\|, \|0\.4\-0\.1\|, \|0\.2\-0\.7\|\) = 0\.5

This tells us the maximum divergence between facet distributions is 0\.5 and occurs in the acceptance rates\. There are three terms in the equation because labels are multiclass of cardinality three\.

The range of LP values for binary, multicategory, and continuous outcomes is \[0, \+1\], where:
+ Values near zero indicate the labels were evenly distributed between facets in all outcome categories\. For example, both facets applying for a loan got 50% of the acceptances and 50% of the rejections\.
+ Values near one indicate the labels for one outcome were all in one facet\. For example, facet *a* got 100% of the acceptances and facet *d* got none\.
+ Intermittent values indicate relative degrees of maximum label imbalance\.