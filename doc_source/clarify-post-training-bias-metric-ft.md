# Counterfactual fliptest \(FT\)<a name="clarify-post-training-bias-metric-ft"></a>

The fliptest is an approach that looks at each member of the disadvantaged group and assesses whether similar members of the advantaged group have different model predictions\. The members of the advantaged group are chosen to be k\-nearest neighbors of the observation from the disadvantaged group\. We assess how many nearest neighbors of the opposite group receive a different prediction, where the flipped prediction can go from positive to negative and vice versa\. 

The formula for the counterfactual fliptest is the difference in the cardinality of two sets divided by the number of members of the advantaged facet:

        FT = \(F\+ \- F\-\)/nd

where:
+ F\+ = is the number of disadvantaged group members with an unfavorable outcome whose nearest neighbors in the advantaged group received a favorable outcome\. 
+ F\- = is the number of disadvantaged group members with a favorable outcome whose nearest neighbors in the advantaged group received an unfavorable outcome\. 

The range of values for the counterfactual fliptest for binary and multi\-category facet labels is \[\-1, \+1\]\. The FT metric is not defined for continuous labels\. 
+ Positive values occur when the number of unfavorable counterfactual fliptest decisions for the disadvantaged group exceeds the favorable ones\. 
+ Values near zero occur when the number of unfavorable and favorable counterfactual fliptest decisions balance out\.
+ Negative values occur when when the number of unfavorable counterfactual fliptest decisions for the disadvantaged group is less than the favorable ones\.