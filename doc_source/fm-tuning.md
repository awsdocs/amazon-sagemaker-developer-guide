# Tune a Factorization Machines Model<a name="fm-tuning"></a>

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the Factorization Machines Algorithm<a name="fm-metrics"></a>

The factorization machines algorithm has both binary classification and regression predictor types\. The predictor type determines which metric you can use for automatic model tuning\. The algorithm reports a `test:rmse` regressor metric, which is computed during training\. When tuning the model for regression tasks, choose this metric as the objective\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:rmse | Root Mean Square Error | Minimize | 

The factorization machines algorithm reports three binary classification metrics, which are computed during training\. When tuning the model for binary classification tasks, choose one of these as the objective\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:binary\_classification\_accuracy | Accuracy | Maximize | 
| test:binary\_classification\_cross\_entropy | Cross Entropy | Minimize | 
| test:binary\_f\_beta | Beta | Maximize | 

## Tunable Factorization Machines Hyperparameters<a name="fm-tunable-hyperparameters"></a>

You can tune the following hyperparameters for the factorization machines algorithm\. The initialization parameters that contain the terms bias, linear, and factorization depend on their initialization method\. There are three initialization methods: `uniform`, `normal`, and `constant`\. These initialization methods are not themselves tunable\. The parameters that are tunable are dependent on this choice of the initialization method\. For example, if the initialization method is `uniform`, then only the `scale` parameters are tunable\. Specifically, if `bias_init_method==uniform`, then `bias_init_scale`, `linear_init_scale`, and `factors_init_scale` are tunable\. Similarly, if the initialization method is `normal`, then only `sigma` parameters are tunable\. If the initialization method is `constant`, then only `value` parameters are tunable\. These dependencies are listed in the following table\. 


| Parameter Name | Parameter Type | Recommended Ranges | Dependency | 
| --- | --- | --- | --- | 
| bias\_init\_scale | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==uniform | 
| bias\_init\_sigma | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==normal | 
| bias\_init\_value | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==constant | 
| bias\_lr | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | None | 
| bias\_wd | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | None | 
| epoch | IntegerParameterRange | MinValue: 1, MaxValue: 1000 | None | 
| factors\_init\_scale | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==uniform | 
| factors\_init\_sigma | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==normal | 
| factors\_init\_value | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==constant | 
| factors\_lr | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | None | 
| factors\_wd | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512\] | None | 
| linear\_init\_scale | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==uniform | 
| linear\_init\_sigma | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==normal | 
| linear\_init\_value | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | bias\_init\_method==constant | 
| linear\_lr | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | None | 
| linear\_wd | ContinuousParameterRange | MinValue: 1e\-8, MaxValue: 512 | None | 
| mini\_batch\_size | IntegerParameterRange | MinValue: 100, MaxValue: 10000 | None | 