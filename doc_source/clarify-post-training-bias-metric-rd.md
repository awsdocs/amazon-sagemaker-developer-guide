# Recall difference \(RD\)<a name="clarify-post-training-bias-metric-rd"></a>

The recall difference \(RD\) metric is the difference in recall of the model between the advantaged facet and the disadvantaged facet\. Any difference in these recalls is a potential form of bias\. Recall is the true positive rate \(TPR\), which measures how often the model correctly predicts the cases that should receive a positive outcome\. Recall is perfect for a facet if all of the y=1 cases are correctly predicted as y’=1 for that facet\. Recall is greater when the model minimizes false negatives known as the Type II error\. If recall is high for lending to non\-minorities but low for lending to minorities, then the difference provides a measure of this bias against the minorities\. 

The formula for difference in the recall rates for the advantaged and disadvantaged facets:

        RD = TPa/\(TPa \+ FNa\) \- TPd/\(TPd \+ FNd\) = TPRa \- TPRd 

where:
+ TPa are the true positives predicted for the advantaged facet\.
+ FNa are the false positives predicted for the advantaged facet\.
+ TPd are the true positives predicted for the disadvantaged facet\.
+ FNd are the false positives predicted for the disadvantaged facet\.
+ TPRa = TPa/\(TPa \+ FNa\) is the recall for the advantaged facet or its true positive rate
+ TPRd TPd/\(TPd \+ FNd\) is the recall for the disadvantaged facet or its true positive rate

For example, consider the following confusion matrices for the advantaged and disadvantaged facets


**Table: Confusion matrix the advantaged facet a**  

| Class a predictions | Actual outcome 0 | Actual outcome 1 | Total  | 
| --- | --- | --- | --- | 
| 0 | 20 | 5 | 25 | 
| 1 | 10 | 65 | 75 | 
| Total | 30 | 70 | 100 | 


**Table: Confusion matrix the disadvantaged facet d**  

| Class d predictions | Actual outcome 0 | Actual outcome 1 | Total  | 
| --- | --- | --- | --- | 
| 0 | 18 | 7 | 25 | 
| 1 | 5 | 20 | 25 | 
| Total | 23 | 27 | 50 | 

The value of the recall difference is RD = 65/70 \- 20/27 = 0\.93 \- 0\.74 = 0\.19 which indicates a bias against the disadvantaged facet\.

The range of values for the recall difference between the advantaged and disadvantaged facets for binary and multi\-category classification is \[\-1, \+1\]\. This metric is not available for the case of continuous labels\.
+ Positive values are obtained when there is higher recall for the advantaged facet than for the disadvantaged facet\. This suggests that the model finds more of the true positives for the advantaged facet than for the disadvantaged facet, which is a form of bias\. 
+ Values near zero indicate that the recall for the disadvantaged and advantaged facets is similar\. This suggests that the model finds about the same number of true positives in both of these facets and is not biased\.
+ Negative values are obtained when there is higher recall for the disadvantaged facet than for the advantaged facet\. This suggests that the model finds more of the true positives for the disadvantaged facet than for the advantaged facet, which is a form of bias\. 