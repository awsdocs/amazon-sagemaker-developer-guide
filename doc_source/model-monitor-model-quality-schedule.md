# Schedule Model Quality Monitoring Jobs<a name="model-monitor-model-quality-schedule"></a>

For general information about scheduling monitoring jobs, see [Schedule monitoring jobs](model-monitor-scheduling.md)\. For model quality monitoring, you also have to consider the fact that the availability of ground truth labels might be delayed\.

**Topics**

To address this, use offsets\. Model quality jobs include `StartTimeOffset` and `EndTimeOffset`, which are fields of the `ModelQualityJobInput` parameter of the `create_model_quality_job_definition` method that work as follows:
+ `StartTimeOffset` \- If specified, jobs subtract this time from the start time\.
+ `EndTimeOffset` \- If specified, jobs subtract this time from the end time\.

The format of the offsets are, for example, \-PT7H, where 7H is 7 hours\. You can use \-PT\#H or \-P\#D, where H=hours, D=days, and M=minutes, and \# is the number\. In addition, the offset should be in [ISO 8601 duration format](https://en.wikipedia.org/wiki/ISO_8601#Durations)\.

For example, if your ground truth starts coming in after 1 day, but is not complete for a week, set `StartTimeOffset` to `-P8D` and `EndTimeOffset` to `-P1D`\. Then, if you schedule a job to run at `2020-01-09T13:00`, it analyzes data from between `2020-01-01T13:00` and `2020-01-08T13:00`\.

**Important**  
The schedule cadence should be such that one execution finishes before the next execution starts, which allows the ground truth merge job and monitoring job from the execution to complete\. The maximum runtime of an execution is divided between the two jobs, so for an hourly model quality monitoring job, the value of `MaxRuntimeInSeconds` specified as part of `StoppingCondition` should be no more than 1800\.