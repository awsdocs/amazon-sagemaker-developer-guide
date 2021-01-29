# Jensen\-Shannon Divergence \(JS\)<a name="clarify-data-bias-metric-jensen-shannon-divergence"></a>

The Jensen\-Shannon divergence \(JS\) measures how much the label distributions of different facets diverge from each other entropically\. It is based on the Kullback\-Leibler divergence, but it is symmetric\. 

The formula for the Jensen\-Shannon divergence is as follows:

        JS = ½\*\[KL\(Pa \|\| P\) \+ KL\(Pd \|\| P\)\]

Where P = ½\( Pa \+ Pd \), the average label distribution across facets *a* and *d*\.

The range of JS values for binary, multicategory, continuous outcomes is \[0, \+∞\)\.
+ Values near zero mean the labels are similarly distributed\.
+ Positive values mean the label distributions diverge, the more positive the larger the divergence\.

This metric indicates whether there is a big divergence in one of the labels across facets\.  