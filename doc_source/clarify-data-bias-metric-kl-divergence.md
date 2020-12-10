# Kullback\-Leibler divergence \(KL\)<a name="clarify-data-bias-metric-kl-divergence"></a>

The Kullback\-Leibler divergence \(KL\) measures how much the observed label distribution of the advantaged facet Pa\(y\) diverges from distribution of the disadvantaged facet Pd\(y\)\. It is also known as the relative entropy of Pa\(y\) with respect to Pd\(y\) and quantifies the amount of information lost when moving from Pa\(y\) to Pd\(y\)\.

The formula for the Kullback\-Leibler divergence: 

        KL\(Pa \|\| Pd\) = ∑yPa\(y\)\*log\[Pa\(y\)/Pd\(y\)\]

It is the expectation of the logarithmic difference between the probabilities Pa\(y\) and Pd\(y\), where the expectation is weighted by the probabilities Pa\(y\)\. This is not a true distance between the distributions as it is asymmetric and does not satisfy the triangle inequality\. The implementation uses natural logarithms, giving KL in units of nats\. Using different logarithmic bases gives proportional results but in different units\. For example, using base 2 gives KL in units of bits\.

For example, assume that the minority applicants for loans have a 30% approval rate and that this approval rate for non\-minorities is 80%\.Then the Kullback\-Leibler formula gives us the label distribution divergence of the advantaged facet from the disadvantaged facet\.

        KL = 0\.8\*ln\(0\.8/0\.3\) \+ 0\.2\*ln\(0\.2/0\.7\) = 0\.53

There are two terms in the formula here because labels are binary in this example\. But this measure can be applied to multiple label types as well as to binary ones\. For example, in a college admissions scenario, assume an applicant may be assigned one of three category labels: yi = \{y0, y1, y2\} = \{rejected, wait\-listed, accepted\}\. 

Range of values for the KL metric for binary, multi\-category, and continuous outcomes is \[0, \+∞\]\.
+ Values near zero mean the outcomes are similarly distributed in the advantaged and disadvantages facets\.
+ Positive values mean the label distributions diverge, the more positive the larger the divergence\.