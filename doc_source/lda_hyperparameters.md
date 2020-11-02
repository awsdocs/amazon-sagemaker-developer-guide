# LDA Hyperparameters<a name="lda_hyperparameters"></a>

In the `CreateTrainingJob` request, you specify the training algorithm\. You can also specify algorithm\-specific hyperparameters as string\-to\-string maps\. The following table lists the hyperparameters for the LDA training algorithm provided by Amazon SageMaker\. For more information, see [How LDA Works](lda-how-it-works.md)\.


| Parameter Name | Description | 
| --- | --- | 
| num\_topics |  The number of topics for LDA to find within the data\. **Required** Valid values: positive integer  | 
| feature\_dim |  The size of the vocabulary of the input document corpus\. **Required** Valid values: positive integer  | 
| mini\_batch\_size |  The total number of documents in the input document corpus\. **Required** Valid values: positive integer  | 
| alpha0 |  Initial guess for the concentration parameter: the sum of the elements of the Dirichlet prior\. Small values are more likely to generate sparse topic mixtures and large values \(greater than 1\.0\) produce more uniform mixtures\.  **Optional** Valid values: Positive float Default value: 1\.0  | 
| max\_restarts |  The number of restarts to perform during the Alternating Least Squares \(ALS\) spectral decomposition phase of the algorithm\. Can be used to find better quality local minima at the expense of additional computation, but typically should not be adjusted\.  **Optional** Valid values: Positive integer Default value: 10  | 
| max\_iterations |  The maximum number of iterations to perform during the ALS phase of the algorithm\. Can be used to find better quality minima at the expense of additional computation, but typically should not be adjusted\.  **Optional** Valid values: Positive integer Default value: 1000  | 
| tol |  Target error tolerance for the ALS phase of the algorithm\. Can be used to find better quality minima at the expense of additional computation, but typically should not be adjusted\.  **Optional** Valid values: Positive float Default value: 1e\-8  | 