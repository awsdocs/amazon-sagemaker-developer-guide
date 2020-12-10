# Class imbalance \(CI\)<a name="clarify-bias-metric-class-imbalance"></a>

Class imbalance \(CI\) bias occurs when one facet is under\-represented in the dataset\. Typically it is the facet we are calling disadvantaged that has fewer members and that ML algorithms may not learn about accurately during training\. This is because models preferentially fit the larger facets at the expense of the smaller facets and so result in a higher training error for the disadvantaged facet\. But models are also at higher risk of overfitting the smaller classes, which can cause a larger test error for the disadvantaged facet\. As an example, models for granting small business loans may be biased against women because the historical record of loan approvals contains very few women due to them not having applied for loans to start small businesses as frequently as men have in the past\. 

The formula for the \(normalized\) facet imbalance measure:

        CI = \(na \- nd\)/\(na \+ nd\)

where na is the number of members of the advantaged facet and nd the number for the disadvantaged facet\. Its values range over the interval \(\-1, 1\)\. 
+ Positive CI values indicate that the advantaged facet is over\-represented and a value of 1 indicates the data only contains members of the advantaged facet\.
+ Negative values indicate that the advantaged facet is under\-represented and a value of \-1 indicates the data only contains members of the disadvantaged facet\.
+ CI values near either of the extremes values of \-1 or 1 are very imbalanced and are at a substantial risk of making biased predictions\.
+  Values near zero indicate a more equal distribution of members between facets and a value of zero indicates a perfectly equal partition between facets and represents the golden truth for a fair distribution of data before training\.

If a significant facet imbalance is found to exist amoung the facets, you may wish to rebalance the sample before proceeding to train models on it\.