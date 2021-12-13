# Activate time series forecasting<a name="canvas-set-up-forecast"></a>

To give your users the ability to perform time series analyses in Amazon SageMaker Canvas, use the following procedure\.

To give your users the ability to do time series forecasting, do the following\.

1. Add the `AmazonForecastFullAccess` AWS managed policy to your IAM role\.

1. Add the following trust relationship to the IAM role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
            "sagemaker.amazonaws.com", # Already present
            "forecast.amazonaws.com"   # This needs to be added
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

For information about AWS managed policies, see [Managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html)\.