# Recall Difference \(RD\)<a name="clarify-post-training-bias-metric-rd"></a>

The recall difference \(RD\) metric is the difference in recall of the model between the favored facet *a* and disfavored facet *d*\. Any difference in these recalls is a potential form of bias\. Recall is the true positive rate \(TPR\), which measures how often the model correctly predicts the cases that should receive a positive outcome\. Recall is perfect for a facet if all of the y=1 cases are correctly predicted as y’=1 for that facet\. Recall is greater when the model minimizes false negatives known as the Type II error\. For example, how many of the people in two different groups \(facets *a* and *d*\) that should qualify for loans are detected correctly by the model? If the recall rate is high for lending to facet *a*, but low for lending to facet *d*, the difference provides a measure of this bias against the group belonging to facet *d*\. 

The formula for difference in the recall rates for facets *a* and *d*:

        RD = TPa/\(TPa \+ FNa\) \- TPd/\(TPd \+ FNd\) = TPRa \- TPRd 

Where:
+ TPa are the true positives predicted for facet *a*\.
+ FNa are the false negatives predicted for facet *a*\.
+ TPd are the true positives predicted for facet *d*\.
+ FNd are the false negatives predicted for facet *d*\.
+ TPRa = TPa/\(TPa \+ FNa\) is the recall for facet *a*\. or its true positive rate\.
+ TPRd TPd/\(TPd \+ FNd\) is the recall for facet *d*\. or its true positive rate\.

For example, consider the following confusion matrices for facets *a* and *d*\.


**Confusion Matrix for the Favored Facet a**  

| Class a predictions | Actual outcome 0 | Actual outcome 1 | Total  | 
| --- | --- | --- | --- | 
| 0 | 20 | 5 | 25 | 
| 1 | 10 | 65 | 75 | 
| Total | 30 | 70 | 100 | 


**Confusion Matrix for the Disfavored Facet d**  

| Class d predictions | Actual outcome 0 | Actual outcome 1 | Total  | 
| --- | --- | --- | --- | 
| 0 | 18 | 7 | 25 | 
| 1 | 5 | 20 | 25 | 
| Total | 23 | 27 | 50 | 

The value of the recall difference is RD = 65/70 \- 20/27 = 0\.93 \- 0\.74 = 0\.19 which indicates a bias against facet *d*\.

The range of values for the recall difference between facets *a* and *d* for binary and multicategory classification is \[\-1, \+1\]\. This metric is not available for the case of continuous labels\.
+ Positive values are obtained when there is higher recall for facet *a* than for facet *d*\. This suggests that the model finds more of the true positives for facet *a* than for facet *d*, which is a form of bias\. 
+ Values near zero indicate that the recall for facets being compared is similar\. This suggests that the model finds about the same number of true positives in both of these facets and is not biased\.
+ Negative values are obtained when there is higher recall for facet *d* than for facet *a*\. This suggests that the model finds more of the true positives for facet *d* than for facet *a*, which is a form of bias\. 