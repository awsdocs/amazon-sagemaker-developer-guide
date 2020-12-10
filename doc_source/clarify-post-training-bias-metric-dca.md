# Difference in conditional acceptance \(DCAcc\)<a name="clarify-post-training-bias-metric-dca"></a>

This metric compares the observed labels to the labels predicted by the model and assesses whether this is the same across facets for predicted positive outcomes \(acceptances\)\. This metric comes close to mimicking human bias in that it quantifies how many more positive outcomes a model predicted \(labels y’\) to a certain facet as compared to what was observed in the training dataset suggested \(labels y\)\. For example, if there were more acceptances for loan applications for women than predicted by the model based on qualifications as compared to men, this may indicate potential bias in the way loans were approved\.

The formula for the difference in conditional acceptance:

        DCAcc = ca \- cd

where:
+ ca = na\(1\)/ n'a\(1\) is the ratio of the observed number of positive outcomes of value 1 \(acceptances\) of the advantaged facet *a* to the predicted number of positive outcome \(acceptances\) of the advantaged facet\. 
+ cd = nd\(1\)/ n'd\(1\) is the ratio of the observed number of positive outcomes of value 1 \(acceptances\) of the disadvantaged facet *a* to the predicted number of predicted positive outcomes \(acceptances\) of the disadvantaged facet\. 

The DCAcc metric can capture both positive and negative biases that reveal preferential treatment based on qualifications\. Consider the following instances of gender bias on loan rejections\.

Example 1: Positive bias\. Suppose we have data set of 100 men and 50 women who applied for loans where the model recommended that 60 men and 30 women be given loans\. So the predicted proportions are DPPL\-fair, but the observed labels show that 70 men and 20 women were granted loans\. In other words, the model granted loans to 17% more men than the observed labels in the training data suggested \(70/60 = 1\.17\) and granted loans to 33% less women than the observed labels suggested \(20/30 = 0\.67\)\. The calculation of the DCAcc value quantifies this difference between \+17% and \-33%\. The calculation of the DCAcc value gives

        DCAcc = 70/60 \- 20/30 = 1/2

Example 2: Negative bias\. Suppose we have data set of 100 men and 50 women who applied for loans where the model recommended that 60 men and 30 women be given loans, so the predicted proportions are DPPL\-fair, but the observed labels show that 50 men and 40 women were granted loans\. In other words, the model granted loans to 17% fewer men than the observed labels in the training data suggested \(50/60 = 0\.83\), and granted loans to 33% more women than the observed labels suggested \(40/30 = 1\.33\)\. The calculation of the DCAcc value quantifies this difference between \-17% and \+33%\. The calculation of the DCAcc value gives

        DCAcc = 50/60 \- 40/30 = \-1/2

Note that DCAcc can also help detect potential \(unintentional\) biases by humans overseeing the model predictions in a human\-in\-the\-loop setting\. Assume, for example, that the predictions y' by the model were fair, but the eventual decision is made by a human \(possibly with access to additional features\) who can alter the model predictions to generate a new and final version of y'\. The additional processing by the human may unintentionally deny loans to a disproportionate number from one facet\. DCAcc can help detect such potential biases\.

The range of values for differences in conditional acceptance for binary, multi\-category facet, and continuous labels is \(\-∞, \+∞\)\.
+ Positive values occur when the ratio of the observed number of acceptances compared to predicted acceptances for the advantaged facet is higher than that ratio for the disadvantaged facet\. These values indicate a possible bias against the qualified applicants from the disadvantaged facet\. The larger the difference of the ratios, the more extreme the prima facie bias\.
+ Values near zero occur when the ratio of the observed number of acceptances compared to predicted acceptances for the advantaged facet is the similar to the ratio for the disadvantaged facet\. These values indicate that predicted acceptance rates are consistent with the observed values in the labeled data and that the qualified applicants from both advantaged and disadvantaged facets are being rejected in a similar way\. 
+ Negative values occur when the ratio of the observed number of acceptances compared to predicted acceptances for the advantaged facet is less than that ratio for the disadvantaged facet\. These values indicate a possible bias against the qualified applicants from the advantaged facet\. The more negative the difference in the ratios, the more extreme the apparent bias\.