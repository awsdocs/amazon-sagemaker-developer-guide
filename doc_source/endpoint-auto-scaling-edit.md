# Edit a Scaling Policy<a name="endpoint-auto-scaling-edit"></a>

You can edit a variant scaling policy with the AWS Management Console, the AWS CLI, or the Application Auto Scaling API\.

## Edit a Scaling Policy \(Console\)<a name="endpoint-auto-scaling-edit-console"></a>

 To edit a scaling policy with the AWS Management Console, use the same procedure that you used to [Configure Automatic Scaling for a Production Variant \(Console\)](endpoint-auto-scaling-add-policy.md#endpoint-auto-scaling-add-console)\.

## Edit a Scaling Policy \(AWS CLI or Application Auto Scaling API\)<a name="endpoint-auto-scaling-edit-code"></a>

You can use the AWS CLI or the Application Auto Scaling API to edit a scaling policy in the same way that you apply a scaling policy:
+ With the AWS CLI, specify the name of the policy that you want to edit in the `--policy-name` parameter\. Specify new values for the parameters that you want to change\.
+ With the Application Auto Scaling API, specify the name of the policy that you want to edit in the `PolicyName` parameter\. Specify new values for the parameters that you want to change\.

For more information, see [Apply a Scaling Policy to a Production Variant](endpoint-auto-scaling-add-policy.md#endpoint-auto-scaling-add-code-apply)\.