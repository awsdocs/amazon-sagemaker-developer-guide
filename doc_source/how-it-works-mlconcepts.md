# Machine Learning with Amazon SageMaker<a name="how-it-works-mlconcepts"></a>

This section describes a typical machine learning workflow and summarizes how you accomplish those tasks with Amazon SageMaker\. 

In machine learning, you "teach" a computer to make predictions, or inferences\. First, you use an algorithm and example data to train a model\. Then you integrate your model into your application to generate inferences in real time and at scale\. In a production environment, a model typically learns from millions of example data items and produces inferences in hundreds to less than 20 milliseconds\. 

The following diagram illustrates the typical workflow for creating a machine learning model:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ml-concepts-10.png)

 As the diagram illustrates, you typically perform the following activities:

1. **Generate example data**—To train a model, you need example data\. The type of data that you need depends on the business problem that you want the model to solve \(the inferences that you want the model to generate\)\. For example, suppose you want to create a model to predict a number given an input image of a handwritten digit\. To train such a model, you need example images of handwritten numbers\. 

   Data scientists often spend a lot of time exploring and preprocessing, or "wrangling," example data before using it for model training\. To preprocess data, you typically do the following: 

   1. **Fetch the data**— You might have in\-house example data repositories, or you might use datasets that are publicly available\. Typically, you pull the dataset\(s\) into a single repository\. 

   1. **Clean the data**—To improve model training, inspect the data and clean it up as needed\. For example, if your data has a `country name` attribute with values `United States` and `US`, you might want to edit the data to be consistent\. 

   1. **Prepare or transform the data**—You might perform additional data transformations to improve performance\. For example, you might choose to combine attributes\. If your model predicts the conditions that require de\-icing an aircraft, instead of using temperature and humidity attributes separately, you might combine them into a new attribute to get a better model\. 

   In Amazon SageMaker, you preprocess example data in a Jupyter notebook on your notebook instance\. You use your notebook to fetch your dataset, explore, it and prepare it for model training\. For more information, see [Notebook Instances and Notebooks](how-it-works-notebooks-instances.md)\. For more information about data preparation in AWS Marketplace, see [data preparation](https://aws.amazon.com/marketplace/search/results?searchTerms=data+preparation&spellCheck=false&page=1)\. 

1. **Train a model**—Model training includes both training and evaluating the model, as follows: 

   + **Training the model**— To train a model, you need an algorithm\. The algorithm you choose depends on a number of factors\. For a quick, out\-of\-the\-box solution, you might be able to use one of the algorithms that Amazon SageMaker provides\. For a list of algorithms provided by Amazon SageMaker and related considerations, see [Using Built\-in Algorithms with Amazon SageMaker](algos.md)\.

      

     You also need compute resources for training\. Depending on the size of your training dataset and how quickly you need the results, you can use resources ranging from a single, small general purpose instance to a distributed cluster of GPU instances\. For more information, see [Training a Model with Amazon SageMaker ](how-it-works-training.md)\.

      

   + **Evaluating the model**—After you've trained your model, you evaluate it to determine whether the accuracy of the inferences is acceptable\. In Amazon SageMaker, you use either the AWS SDK for Python \(Boto\) or the high\-level Python library that Amazon SageMaker provides to send requests to the model for inferences\. 

     You use a Jupyter notebook in your Amazon SageMaker notebook instance to train and evaluate your model\. 

1. **Deploy the model**— Traditionally, you do some re\-engineering of a model to integrate it with your application, before deploying the model into production\. With Amazon SageMaker hosting services, you can deploy your model independently, decoupling it from your application code\. For more information, see [Deploying a Model on Amazon SageMaker Hosting Services](how-it-works-hosting.md)\.

Machine learning is a continuous cycle\. After deploying a model, you monitor the inferences, then collect "ground truth," and evaluate the model to identify drift\. You then increase the accuracy of your inferences by updating your training data to include the newly collected ground truth, by retraining the model with the new dataset\. As more and more example data becomes available, you continue retraining your model to increase accuracy over time\.

## How It Works: Next Topic<a name="howitwork-mlconcepts-nextstep"></a>

 [Training a Model with Amazon SageMaker ](how-it-works-training.md) 