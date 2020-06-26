# Get Started<a name="multiple-algorithm-hpo-get-started"></a>

**Using Multi\-Algorithm HPO**  
 To use multi\-algorithm HPO you must add more than one training definition to your hyperparameter tuning job\. Each training definition holds the configuration options for each algorithm you want to try\.  

 In the console, you add training definitions when you create the HPO tuning job by choosing **Add training definition**, and then following the configuration steps for each algorithm that you want to use\. 

 When you start the configuration steps, please note that the warm start and early stopping features are not available with multi\-algorithm HPO\. If you want to use these features, you can only tune a single algorithm at a time\. 

 If you’re using an API request, instead of the single `TrainingJobDefinition`, you must provide a list of training definitions using `TrainingJobDefinitions`\. You must use one or the other, not both\. 