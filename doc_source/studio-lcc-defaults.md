# Setting Default Lifecycle Configurations<a name="studio-lcc-defaults"></a>

To set a Lifecycle Configuration as the default for your Domain or UserProfile programatically, you can create a new resource or update an existing resource\. To associate a Lifecycle Configuration as a default, you'll first need to create a Lifecycle Configuration following the steps in [Creating and Associating a Lifecycle Configuration](studio-lcc-create.md)\. Default Lifecycle Configurations set up at the domain level are inherited by all users, while those set up at the user level are scoped to a specific user\. 

**Note**  
User level defaults override defaults set up at the domain level\.

To set up a default Lifecycle Configuration, it must be added to the `DefaultResourceSpec` of the appropriate app type\. The behavior of your Lifecycle Configuration depends on whether it is added to the `DefaultResourceSpec` of a JupyterServer or KernelGateway app\. 
+ **JupyterServer apps:** When added to the `DefaultResourceSpec` of a JupyterServer app, the default Lifecycle Configuration script runs automatically when the user logs into Studio for the first time or restarts Studio\. This can be used to automate one\-time set\-up actions for the Studio developer environment, such as installing notebook extensions or setting up a GitHub repo\. For an example of this, see [Customize Amazon SageMaker Studio using Lifecycle Configurations](http://aws.amazon.com/blogs/machine-learning/customize-amazon-sagemaker-studio-using-lifecycle-configurations/)\.
+ **KernelGateway apps:** When added to the `DefaultResourceSpec` of a KernelGateway app, Studio defaults to selecting the Lifecycle Configuration script from the Studio launcher\. Users can launch a notebook or terminal with the default script selected or they can select a different one from the list of Lifecycle Configurations\.

**Note**  
A default KernelGateway Lifecycle Configuration specified in `DefaultResourceSpec` applies to all KernelGateway images in the Studio Domain unless the user selects a different script from the list presented in the Studio launcher\. The default script also runs if `No Script` is selected by the user\. For more information on selecting a script, see [Step 3: Launch an application with the Lifecycle Configuration](studio-lcc-create-console.md#studio-lcc-create-console-step3)\.

**Associate a default Lifecycle Configuration when creating a new Domain or UserProfile**

To associate a Lifecycle Configuration when creating a new Studio Domain or UserProfile, you need the ARN of the Lifecycle Configuration that you created\. This ARN is passed to one of the following API calls:
+ [create\-user\-profile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html)
+ [create\-domain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html)

For example, the following API call creates a new UserProfile with an associated Lifecycle Configuration\.

```
aws sagemaker create-user-profile --domain-id <DOMAIN-ID> \
--user-profile-name <USER-PROFILE-NAME> \
--region <REGION> \
--user-settings '{
"KernelGatewayAppSettings": {
    "DefaultResourceSpec": { 
            "InstanceType": "ml.t3.medium",
            "LifecycleConfigArn": "<LIFECYCLE-CONFIGURATION-ARN>"
         }
  }
}'
```

**Associate a default Lifecycle Configuration when updating a Domain or UserProfile**

To associate a Lifecycle Configuration when updating an existing Studio Domain or UserProfile, you need the ARN of the Lifecycle Configuration that you created\. This ARN is passed to one of the following API calls:
+ [update\-user\-profile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateUserProfile.html)
+ [update\-domain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateDomain.html)

The Lifecycle Configuration ARN should be placed in 2 places, the `DefaultResourceSpec` and the `LifecycleConfigArns` list in `KernelGatewayAppSettings`\. For example, the following API call updates a UserProfile with an associated Lifecycle Configuration\.

```
aws sagemaker update-user-profile --domain-id d-cnfvrxpcrsvs \
--user-profile-name portfolio-data-dev \
--user-settings '{
	"KernelGatewayAppSettings": {
		"DefaultResourceSpec": {
			"InstanceType": "ml.t3.medium",
			"LifecycleConfigArn": "<LIFECYCLE-CONFIGURATION-ARN>"
		},
		"LifecycleConfigArns": [
			"<LIFECYCLE-CONFIGURATION-ARN>"
		]
	}
}'
```
