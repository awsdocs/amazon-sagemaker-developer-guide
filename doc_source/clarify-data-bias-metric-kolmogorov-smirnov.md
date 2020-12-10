# Kolmogorov\-Smirnov \(KS\)<a name="clarify-data-bias-metric-kolmogorov-smirnov"></a>

The Kolmogorov\-Smirnov bias metric \(KS\) is equal to the maximum divergence between labels in distributions for the advantaged and disadvantaged facets of a data set\. The two\-sample KS test implemented by SageMaker Clarify complements the other measures of label imbalance by finding the most imbalanced label\. 

The formula for the Kolmogorov\-Smirnov metric: 

        KS = max\(\|Pa\(y\) \- Pd\(y\)\|\)

For example, assume the minority applicants to college are rejected, wait\-listed, or accepted at 40%, 40%, 20% respectively and that these rates for non\-minority applicants are 20%, 10%, 70%\. Then the Kolmogorov\-Smirnov bias metric value is

KS = max\(\|0\.4\-0\.2\|, \|0\.4\-0\.3\|, \|0\.2\-0\.7\|\) = 0\.50

This tells us the maximum divergence between facet distributions occurs in the acceptance rates\. There are three terms in the equation because labels are multiclass of cardinality three\.

The range of LP values for binary, multi\-category, and continuous outcomes is \[0, \+1\]
+ Values near zero indicate the labels were evenly distributed between facets in all categories\. For example, both the advantaged disadvantaged facets applying for a loan got 50% of the acceptances and 50% of the rejections\.
+ Values near one indicate the labels for one category were all in one facet\. For example, the advantaged facet got 100% of the acceptances and the disadvantaged facet got none\.
+ Intermittent values indicate relative degrees of maximum label imbalance, the closer to 1 the greater the maximum label imbalance\.