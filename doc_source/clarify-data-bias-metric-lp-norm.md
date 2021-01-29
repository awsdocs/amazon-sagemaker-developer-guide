# Lp\-norm \(LP\)<a name="clarify-data-bias-metric-lp-norm"></a>

The Lp\-norm \(LP\) measures the p\-norm distance between the facet distributions of the observed labels in a training dataset\. This metric is non\-negative and so cannot detect reverse bias\. 

The formula for the Lp\-norm is as follows: 

        Lp\(Pa, Pd\) = \( ∑y\|\|Pa \- Pd\|\|p\)1/p

Where the p\-norm distance between the points x and y is defined as follows:

        Lp\(x, y\) = \(\|x1\-y1\|p \+ \|x2\-y2\|p \+ … \+\|xn\-yn\|p\)1/p 

The 2\-norm is the Euclidean norm\. Assume you have an outcome distribution with three categories, for example, yi = \{y0, y1, y2\} = \{accepted, waitlisted, rejected\} in a college admissions multicategory scenario\. You take the sum of the squares of the differences between the outcome counts for facets *a* and *d*\. The resulting Euclidean distance is calculated as follows:

        L2\(Pa, Pd\) = \[\(na\(0\) \- nd\(0\)\)2 \+ \(na\(1\) \- nd\(1\)\)2 \+ \(na\(2\) \- nd\(2\)\)2\]1/2

Where: 
+ na\(i\) is number of the ith category outcomes in facet *a*: for example na\(0\) is number of facet *a* acceptances\.
+ nd\(i\) is number of the ith category outcomes in facet *d*: for example nd\(2\) is number of facet *d* rejections\.

  The range of LP values for binary, multicategory, and continuous outcomes is \[0, \+∞\), where:
  + Values near zero mean the labels are similarly distributed\.
  + Positive values mean the label distributions diverge, the more positive the larger the divergence\.