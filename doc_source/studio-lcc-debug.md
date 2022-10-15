# Debugging Lifecycle Configurations<a name="studio-lcc-debug"></a>

The following topics show how to get information about and debug your Lifecycle Configurations\.

**Topics**
+ [Verify Lifecycle Configuration Process from Amazon CloudWatch Logs](#studio-lcc-debug-logs)
+ [JupyterServer App failure](#studio-lcc-debug-jupyterserver)
+ [KernelGateway App failure](#studio-lcc-debug-kernel)
+ [Lifecycle Config timeout](#studio-lcc-debug-timeout)

## Verify Lifecycle Configuration Process from Amazon CloudWatch Logs<a name="studio-lcc-debug-logs"></a>

Lifecycle Configurations only log `STDOUT` and `STDERR`\. `STDOUT` is the default output for bash scripts, while `STDERR` can be written to by appending `>&2` to the end of a bash command\. For example, `echo 'hello'>&2`\. Logs for your Lifecycle Configurations are published to your AWS Account via CloudWatch\. These logs can be found in the `/aws/sagemaker/studio` Log Stream from the AWS CloudWatch console\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Select `Logs` from the left side\. From the dropdown menu, select `Log Groups`\.

1. On the `Log Groups` screen, search for `aws/sagemaker/studio`\. Select the log group\.

1. On the `aws/sagemaker/studio` `Log Group` screen, navigate to the `Log Streams` tab\.

1. To find the logs for a specific app, search `Log Streams` using the following format:

   ```
   <DomainId>/<UserProfileName>/<AppType>/<AppName>
   ```

   For example, to find the Lifecycle Configuration logs for Domain `d-m85lcu8vbqmz`, UserProfile `i-sonic-js`, Apptype `JupyterServer` and AppName `test-lcc-echo`, use the following search string:

   ```
   d-m85lcu8vbqmz/i-sonic-js/JupyterServer/test-lcc-echo 
   ```

1. Select the log stream appended with `LifecycleConfigOnStart` to view the script execution logs\.

## JupyterServer App failure<a name="studio-lcc-debug-jupyterserver"></a>

If your JupyterServer App crashes because of an issue with the attached Lifecycle Configuration, Studio displays the following error message on the Studio startup screen\. 

```
Failed to create SageMaker Studio due to start-up script failure
```

Click the `View script logs` link to view the CloudWatch logs for your JupyterServer app\.

In the case where the faulty Lifecycle Configuration is specified in the `DefaultResourceSpec` of your Studio Domain or UserProfile, Studio continues to use the Lifecycle Configuration even after restarting Studio\. 

To resolve this error, follow the steps in [Setting Default Lifecycle Configurations](studio-lcc-defaults.md) to remove the Lifecycle Configuration script from the `DefaultResourceSpec` or select another script using the AWS CLI\. Then launch a new JupyterServer app\.

## KernelGateway App failure<a name="studio-lcc-debug-kernel"></a>

If your KernelGateway App crashes because of an issue with the attached Lifecycle Configuration, Studio displays the error message in your Studio Notebook\. 

Click the `View script logs` link to view the CloudWatch logs for your KernelGateway app\.

In this case, your Lifecycle Configuration is specified in the Studio Launcher when launching a new Studio Notebook\. 

To resolve this error, use the Studio launcher to select a different Lifecycle Configuration or select `No script`\.

**Note**  
A default KernelGateway Lifecycle Configuration specified in `DefaultResourceSpec` applies to all KernelGateway images in the Studio Domain unless the user selects a different script from the list presented in the Studio launcher\. The default script also runs if `No Script` is selected by the user\. For more information on selecting a script, see [Step 3: Launch an application with the Lifecycle Configuration](studio-lcc-create-console.md#studio-lcc-create-console-step3)\.

## Lifecycle Config timeout<a name="studio-lcc-debug-timeout"></a>

There is a Lifecycle Configuration timeout limitation of 5 minutes\. If a Lifecycle Configuration script takes longer than 5 minutes to run, Studio throws an error\.

To resolve this error, ensure that your Lifecycle Configuration script completes in less than 5 minutes\. 

To help decrease the run time of scripts, try the following:
+ Cut down on necessary steps\. For example, limit which conda environments to install large packages in\.
+ Run tasks in parallel processes\.
+ Use the `nohup` command in your script\.