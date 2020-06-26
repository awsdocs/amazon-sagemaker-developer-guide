# Amazon SageMaker Debugger Best Practices<a name="debugger-best-practices"></a>

**Step Intervals:** By default, the Python SDK uses step interval as 500 for emitting tensors\. If you want to emit more frequently, choose specific tensors and run for fewer epochs to avoid stressing the CPU/disk\. 