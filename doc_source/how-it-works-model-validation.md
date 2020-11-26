# Validate a Machine Learning Model<a name="how-it-works-model-validation"></a>

After training a model, evaluate it to determine whether its performance and accuracy enable you to achieve your business goals\. You might generate multiple models using different methods and evaluate each\. For example, you could apply different business rules for each model, and then apply various measures to determine each model's suitability\. You might consider whether your model needs to be more sensitive than specific \(or vice versa\)\. 

You can evaluate your model using historical data \(offline\) or live data:
+ **Offline testing**—Use historical, not live, data to send requests to the model for inferences\. 

  Deploy your trained model to an alpha endpoint, and use historical data to send inference requests to it\. To send the requests, use a Jupyter notebook in your Amazon SageMaker notebook instance and either the AWS SDK for Python \(Boto\) or the high\-level Python library provided by SageMaker\.
+ **Online testing with live data**—SageMaker supports A/B testing for models in production by using production variants\. Production variants are models that use the same inference code and are deployed on the same SageMaker endpoint\. You configure the production variants so that a small portion of the live traffic goes to the model that you want to validate\. For example, you might choose to send 10% of the traffic to a model variant for evaluation\. After you are satisfied with the model's performance, you can route 100% traffic to the updated model\. For an example of testing models in production, see [Test models in production](model-ab-testing.md)\.

For more information, see articles and books about how to evaluate models, for example, [Evaluating Machine Learning Models](http://www.oreilly.com/data/free/evaluating-machine-learning-models.csp)\. 

Options for offline model evaluation include:
+ **Validating using a holdout set**—Machine learning practitioners often set aside a part of the data as a "holdout set\." They don’t use this data for model training\.

  With this approach, you evaluate how well your model provides inferences on the holdout set\. You then assess how effectively the model generalizes what it learned in the initial training, as opposed to using model memory\. This approach to validation gives you an idea of how often the model is able to infer the correct answer\. 

   

  In some ways, this approach is similar to teaching elementary school students\. First, you provide them with a set of examples to learn, and then test their ability to generalize from their learning\. With homework and tests, you pose problems that were not included in the initial learning and determine whether they are able to generalize effectively\. Students with perfect memories could memorize the problems, instead of learning the rules\.

   

  Typically, the holdout dataset is of 20\-30% of the training data\.

   
+ **k\-fold validation**—In this validation approach, you split the example dataset into *k* parts\. You treat each of these parts as a holdout set for* k* training runs, and use the other *k*\-1 parts as the training set for that run\. You produce* k* models using a similar process, and aggregate the models to generate your final model\. The value *k* is typically in the range of 5\-10\.