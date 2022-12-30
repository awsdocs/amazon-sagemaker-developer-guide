# Blue/Green Deployments<a name="deployment-guardrails-blue-green"></a>

When you update your endpoint, Amazon SageMaker automatically uses a blue/green deployment to maximize the availability of your endpoints\. In a blue/green deployment, SageMaker provisions a new fleet with the updates \(the green fleet\)\. Then, SageMaker shifts traffic from the old fleet \(the blue fleet\) to the green fleet\. Once the green fleet operates smoothly for a set evaluation period \(called the baking period\), SageMaker terminates the blue fleet\. With the additional capabilities in blue/green deployments, you can utilize traffic shifting modes and auto\-rollback monitoring to protect your endpoint from significant production impact\.

The following list describes the key features of blue/green deployments in SageMaker:
+ **Traffic shifting modes\.** The traffic shifting modes for deployment guardrails let you control the volume of traffic and number of traffic\-shifting steps between the blue fleet and the green fleet\. This capability gives you the ability to progressively evaluate the performance of the green fleet without fully committing to a 100% traffic shift\.
+ **Baking period\.** The baking period is a set amount of time to monitor the green fleet before proceeding to the next deployment stage\. If any of the pre\-specified alarms trip during any baking period, then all endpoint traffic rolls back to the blue fleet\. The baking period helps you to build confidence in your update before making the traffic shift permanent\.
+ **Auto\-rollbacks\.** You can specify Amazon CloudWatch alarms that SageMaker uses to monitor the green fleet\. If an issue with the updated code trips any of the alarms, SageMaker initiates an auto\-rollback to the blue fleet in order to maintain availability thereby minimizing risk\.

## Traffic Shifting Modes<a name="deployment-guardrails-blue-green-traffic-modes"></a>

The various traffic shifting modes in blue/green deployments give you more granular control over traffic shifting between the blue fleet and the green fleet\. The available traffic shifting modes for blue/green deployments are all at once, canary, and linear\. The following table shows a comparison of the options\.

**Important**  
For blue/green deployments that involve multiple stage traffic shifting or baking periods, you are billed for both the fleets for the duration of the update, irrespective of the traffic to the fleet\. This is in contrast to blue/green deployments with all at once traffic shifting and no baking periods, where you are only billed for one fleet during the course of the update\.


| Name | What is it? | Pros | Cons | Recommendation | 
| --- | --- | --- | --- | --- | 
| All at once | Shifts all of the traffic to the new fleet in a single step\. | Minimizes the overall update duration\. | Regressive updates affect 100% of the traffic\. | Use this option to minimize update time and cost\. | 
| Canary | Traffic shifts in two steps\. The first \(canary\) step shifts a small portion of the traffic followed by the second step, which shifts the remainder of the traffic\. | Confines the blast radius of regressive updates to only the canary fleet\. | Both fleets are operational in parallel for entire deployment\. | Use this option to balance between minimizing the blast radius of regressive updates and minimizing the time that two fleets are operational\. | 
| Linear | A fixed portion of the traffic shifts in a pre\-specified number of equally spaced steps\. | Minimizes the risk of regressive updates by shifting traffic over several steps\. | The update duration and cost are proportional to the number of steps\. | Use this option to minimize risk by spreading out deployment across multiple steps\. | 

## Get Started<a name="deployment-guardrails-blue-green-get-started"></a>

Once you specify your desired deployment configuration, SageMaker handles provisioning new instances, terminating old instances, and shifting traffic for you\. You can create and manage your deployment through the existing [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) and [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) SageMaker API and AWS Command Line Interface commands\. Note that if your endpoint uses any of the features listed in the [Exclusions](deployment-guardrails-exclusions.md) page, you cannot use deployment guardrails\. See the individual deployment pages for more details on how to set up your deployment:
+ [ Blue/Green Update with All At Once Traffic Shifting](deployment-guardrails-blue-green-all-at-once.md)
+ [ Blue/Green Update with Canary Traffic Shifting](deployment-guardrails-blue-green-canary.md)
+ [ Blue/Green Update with Linear Traffic Shifting](deployment-guardrails-blue-green-linear.md)

To follow guided examples that show how to use deployment guardrails, see our example [Jupyter notebooks](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-inference-deployment-guardrails) for the canary and linear traffic shifting modes\.