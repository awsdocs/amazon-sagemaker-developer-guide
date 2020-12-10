# Lp\-norm \(LP\)<a name="clarify-data-bias-metric-lp-norm"></a>

The Lp\-norm \(LP\) measures the p\-norm distance between the advantaged and disadvantaged distributions of the observed labels in a training data set\. This metric is non\-negative and so cannot detect reverse bias\. 

The formula for the Lp\-norm: 

        Lp\(Pa, Pd\) = \( ∑y\|\|Pa \- Pd\|\|p\)1/p

where the p\-norm distance between the points x and y is defined as

        Lp\(x, y\) = \(\|x1\-y1\|p \+ \|x2\-y2\|p \+ … \+\|xn\-yn\|p\)1/p 

The 2\-norm is the Euclidean norm\. If you have an outcome distribution with three categories, for example, yi = \{y0, y1, y2\} = \{accepted, wait\-listed, rejected\} in a college admissions multi\-category scenario, you take the differences between the counts in each category of the advantaged and disadvantaged facet distributions and then calculate Euclidean distance\. The result is

        L2\(Pa, Pd\) = \[\(na\(0\) \- nd\(0\)\)2 \+ \(na\(1\) \- nd\(1\)\)2 \+ \(na\(2\) \- nd\(2\)\)2\]1/2

where 
+ na\(i\) is number of the ith category in the advantaged facet: for example na\(0\) is number of advantaged facet acceptances
+ nd\(i\) is number of the ith category in the disadvantaged facet: for example nd\(2\) is number of disadvantaged facet rejections

  The range of LP values for binary, multi\-category, and continuous outcomes is \[0, \+∞\]\.
  + Values near zero mean the labels are similarly distributed\.
  + Positive values mean the label distributions diverge, the more positive the larger the divergence\.