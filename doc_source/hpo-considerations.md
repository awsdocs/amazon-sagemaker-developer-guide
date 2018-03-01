# Design Considerations<a name="hpo-considerations"></a>

Hyperparameter optimization is not a fully\-automated process\. There are a number of design considerations that can help improve optimization\.


+ [Choosing the Number of Hyperparameters](#hpo-num-hyperparameters)
+ [Choosing Hyperparameter Ranges](#hpo-choosing-ranges)
+ [Use Logarithmic Scales for Hyperparameters](#hpo-log-scales)
+ [Choosing the Best Degree of Parallelism](#hpo-parallelism)

## Choosing the Number of Hyperparameters<a name="hpo-num-hyperparameters"></a>

The number of hyperparameters to search over is the single biggest factor that determines how difficult your HPO problem is\. While HPO supports up to 20 variables at once, limiting your search to a much smaller number is likely to give better results with a realistic amount of training jobs\.

## Choosing Hyperparameter Ranges<a name="hpo-choosing-ranges"></a>

Choosing ranges for hyperparameters to search over can significantly affect the success of a hyperparameter optimization\. While it is tempting to specify a very large range that covers every possible value, you likely get better results by limiting your search to a small range where you expect all possible values in the range to be reasonable\. If you get best metric values within a small part of a range, consider limiting the boundaries of the range to that part\.

## Use Logarithmic Scales for Hyperparameters<a name="hpo-log-scales"></a>

HPO attempts to ﬁgure out if your hyperparameters are log\-scaled or linear\-scaled, but it assumes variables are linearly\-scaled to start with, and sometimes the process is slow to realize it should be log\-scaled\. If you know a variable should be log\-scaled, and you can do that conversion yourself, that could improve your optimization\. 

## Choosing the Best Degree of Parallelism<a name="hpo-parallelism"></a>

More jobs in parallel gets more work done quickly, but an HPO job can be improved only with successive rounds of experiments\. Typically, running 1 training job at a time achieves the best results with the least amount of compute time\. There is an trade\-oﬀ between minimizing wall\-clock time, and minimizing total compute hours used