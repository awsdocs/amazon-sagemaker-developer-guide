# Generalized entropy \(GE\)<a name="clarify-post-training-bias-metric-ge"></a>

The generalized entropy index \(GE\) measures the inequality in benefit `b` for the predicted label compared to the observed label\. A benefit occurs when a false positive is predicted\. A false positive occurs when a negative observation \(y=0\) has a positive prediction \(y'=1\)\. A benefit also occurs when the observed and predicted labels are the same, also known as a true positive and true negative\. No benefit occurs when a false negative is predicted\. A false negative occurs when a positive observation \(y=1\) is predicted to have a negative outcome \(y'=0\)\. The benefit `b` is defined, as follows\.

```
 b = y' - y + 1
```

Using this definition, a false positive receives a benefit `b` of `2`, and a false negative receives a benefit of `0`\. Both a true positive and a true negative receive a benefit of `1`\.

The GE metric is computed following the [Generalized Entropy Index](https://en.wikipedia.org/wiki/Generalized_entropy_index) \(GE\) with the weight `alpha` set to `2`\. This weight controls the sensitivity to different benefit values\. A smaller `alpha` means an increased sensitivity to smaller values\.

![\[Equation defining generalized entropy index with alpha parameter set to 2.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify-post-training-bias-metric-ge.png)

The following variables used to calculate GE are defined as follows:
+ bi is the benefit received by the `ith` data point\.
+ b' is the mean of all benefits\.

GE can range from 0 to 0\.5, where values of zero indicate no inequality in benefits across all data points\. This occurs either when all inputs are correctly predicted or when all the predictions are false positives\. GE is undefined when all predictions are false negatives\.

**Note**  
The metric GE does not depend on a facet value being either favored or disfavored\.