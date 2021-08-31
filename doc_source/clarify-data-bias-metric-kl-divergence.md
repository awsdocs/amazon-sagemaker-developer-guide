# Kullback\-Leibler Divergence \(KL\)<a name="clarify-data-bias-metric-kl-divergence"></a>

The Kullback\-Leibler divergence \(KL\) measures how much the observed label distribution of facet *a*, Pa\(y\), diverges from distribution of facet *d*, Pd\(y\)\. It is also known as the relative entropy of Pa\(y\) with respect to Pd\(y\) and quantifies the amount of information lost when moving from Pa\(y\) to Pd\(y\)\.

The formula for the Kullback\-Leibler divergence is as follows: 

        KL\(Pa \|\| Pd\) = ∑yPa\(y\)\*log\[Pa\(y\)/Pd\(y\)\]

It is the expectation of the logarithmic difference between the probabilities Pa\(y\) and Pd\(y\), where the expectation is weighted by the probabilities Pa\(y\)\. This is not a true distance between the distributions as it is asymmetric and does not satisfy the triangle inequality\. The implementation uses natural logarithms, giving KL in units of nats\. Using different logarithmic bases gives proportional results but in different units\. For example, using base 2 gives KL in units of bits\.

For example, assume that a group of applicants for loans have a 30% approval rate \(facet *d*\) and that the approval rate for other applicants \(facet *a*\) is 80%\. The Kullback\-Leibler formula gives you the label distribution divergence of facet *a* from facet *d* as follows:

        KL = 0\.8\*ln\(0\.8/0\.3\) \+ 0\.2\*ln\(0\.2/0\.7\) = 0\.53

There are two terms in the formula here because labels are binary in this example\. This measure can be applied to multiple labels in addition to binary ones\. For example, in a college admissions scenario, assume an applicant may be assigned one of three category labels: yi = \{y0, y1, y2\} = \{rejected, waitlisted, accepted\}\. 

Range of values for the KL metric for binary, multicategory, and continuous outcomes is \[0, \+∞\)\.
+ Values near zero mean the outcomes are similarly distributed for the different facets\.
+ Positive values mean the label distributions diverge, the more positive the larger the divergence\.