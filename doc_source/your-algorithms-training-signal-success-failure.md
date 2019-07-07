# How Amazon SageMaker Signals Algorithm Success and Failure<a name="your-algorithms-training-signal-success-failure"></a>

A training algorithm indicates whether it succeeded or failed using the exit code of its process\. 

A successful training execution should exit with an exit code of 0 and an unsuccessful training execution should exit with a non\-zero exit code\. These will be converted to "Completed" and "Failed" in the `TrainingJobStatus` returned by `DescribeTrainingJob`\. This exit code convention is standard and is easily implemented in all languages\. For example, in Python, you can use `sys.exit(1)` to signal a failure exit and simply running to the end of the main routine will cause Python to exit with code 0\.

In the case of failure, the algorithm can write a description of the failure to the failure file\. See next section for details\.