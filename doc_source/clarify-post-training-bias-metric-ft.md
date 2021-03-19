# Counterfactual Fliptest \(FT\)<a name="clarify-post-training-bias-metric-ft"></a>

The fliptest is an approach that looks at each member of facet *d* and assesses whether similar members of facet *a* have different model predictions\. The members of facet *a* are chosen to be k\-nearest neighbors of the observation from facet *d*\. We assess how many nearest neighbors of the opposite group receive a different prediction, where the flipped prediction can go from positive to negative and vice versa\. 

The formula for the counterfactual fliptest is the difference in the cardinality of two sets divided by the number of members of facet *d*:

        FT = \(F\+ \- F\-\)/nd

Where:
+ F\+ = is the number of disfavored facet *d* members with an unfavorable outcome whose nearest neighbors in favored facet *a* received a favorable outcome\. 
+ F\- = is the number of disfavored facet *d* members with a favorable outcome whose nearest neighbors in favored facet *a* received an unfavorable outcome\. 
+ nd is the sample size of facet *d*\.

The range of values for the counterfactual fliptest for binary and multicategory facet labels is \[\-1, \+1\]\. For continuous labels, we set a threshold to collapse the labels to binary\.
+ Positive values occur when the number of unfavorable counterfactual fliptest decisions for the disfavored facet *d* exceeds the favorable ones\. 
+ Values near zero occur when the number of unfavorable and favorable counterfactual fliptest decisions balance out\.
+ Negative values occur when the number of unfavorable counterfactual fliptest decisions for the disfavored facet *d* is less than the favorable ones\.