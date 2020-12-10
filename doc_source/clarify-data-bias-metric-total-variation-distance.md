# Total variation distance \(TVD\)<a name="clarify-data-bias-metric-total-variation-distance"></a>

The total variation distance data bias metric \(TVD\) is half the L1\-norm\. The TVD is the largest possible difference between the probability distributions for label outcomes of the advantaged and disadvantaged facets\. The L1\-norm is the Hamming distance, a metric used compare two binary data strings by determining the minimum number of substitutions required to change one string into another\. If the strings were to be copies of each other, it determines the number of errors that occurred when copying\. In the bias detection context, TVD quantifies how many outcomes in the advantaged facet would have to be changed to match the outcomes in the disadvantaged facet\.

The formula for the Total variation distance: 

        TVD = ½\*L1\(Pa, Pd\)

 If you have an outcome distribution with three categories, for example, yi = \{y0, y1, y2\} = \{accepted, wait\-listed, rejected\} in a college admissions multi\-category scenario, you take the differences between the counts in each category of the advantaged and disadvantaged facet distributions and then calculate TVD\. The result is

        L1\(Pa, Pd\) = \|na\(0\) \- nd\(0\)\| \+ \|na\(1\) \- nd\(1\)\| \+ \|na\(2\) \- nd\(2\)\|

where 
+ na\(i\) is number of the ith category in the advantaged facet: for example na\(0\) is number of advantaged facet acceptances
+ nd\(i\) is number of the ith category in the advantaged facet: for example nd\(2\) is number of disadvantaged facet rejections

  The range of LP values for binary, multi\-category, and continuous outcomes is \[0, \+∞\]\.
  + Values near zero mean the labels are similarly distributed\.
  + Positive values mean the label distributions diverge, the more positive the larger the divergence\.