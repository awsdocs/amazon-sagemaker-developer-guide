# Total Variation Distance \(TVD\)<a name="clarify-data-bias-metric-total-variation-distance"></a>

The total variation distance data bias metric \(TVD\) is half the L1\-norm\. The TVD is the largest possible difference between the probability distributions for label outcomes of facets *a* and *d*\. The L1\-norm is the Hamming distance, a metric used compare two binary data strings by determining the minimum number of substitutions required to change one string into another\. If the strings were to be copies of each other, it determines the number of errors that occurred when copying\. In the bias detection context, TVD quantifies how many outcomes in facet *a* would have to be changed to match the outcomes in facet *d*\.

The formula for the Total variation distance is as follows: 

 TVD = ½\*L1\(Pa, Pd\)

For example, assume you have an outcome distribution with three categories, yi = \{y0, y1, y2\} = \{accepted, waitlisted, rejected\}, in a college admissions multicategory scenario\. You take the differences between the counts of facets *a* and *d* for each outcome to calculate TVD\. The result is as follows:

 L1\(Pa, Pd\) = \|na\(0\) \- nd\(0\)\| \+ \|na\(1\) \- nd\(1\)\| \+ \|na\(2\) \- nd\(2\)\|

Where: 
+ na\(i\) is number of the ith category outcomes in facet *a*: for example na\(0\) is number of facet *a* acceptances\.
+ nd\(i\) is number of the ith category outcomes in facet d: for example nd\(2\) is number of facet *d* rejections\.

  The range of TVD values for binary, multicategory, and continuous outcomes is \[0, 1\), where:
  + Values near zero mean the labels are similarly distributed\.
  + Positive values mean the label distributions diverge, the more positive the larger the divergence\.