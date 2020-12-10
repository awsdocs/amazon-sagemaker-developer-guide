# Jensen\-Shannon divergence \(JS\)<a name="clarify-data-bias-metric-jensen-shannon-divergence"></a>

The Jensen\-Shannon divergence \(JS\) measures how much the label distributions of different facets diverge from each other entropically\. It is based on the Kullback\-Leibler divergence but is symmetric\. 

The formula for the Jensen\-Shannon divergence:

        JS = ½\*\[KL\(Pa \|\| P\) \+ KL\(Pd \|\| P\]

where P = ½\( Pa \+ Pd \), the average label distribution across the advantaged and disadvantaged facets\.

The range of JS values for binary, multi\-category, continuous outcomes is \[0, \+∞\]\.
+ Values near zero mean the labels are similarly distributed\.
+ Positive values mean the label distributions diverge, the more positive the larger the divergence\.

This metric indicates whether there is a big divergence in one of the labels across facets\.  