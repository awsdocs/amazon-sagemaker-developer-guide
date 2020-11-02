# Multi\-Model Endpoint Security<a name="multi-model-endpoint-security"></a>

Models and data in a multi\-model endpoint are co\-located on instance storage volume and in container memory\. All instances for Amazon SageMaker endpoints run on a single tenant container that you own\. Only your models can run on your multi\-model endpoint\. It's your responsibility to manage the mapping of requests to models and to provide access for users to the correct target models\. SageMaker uses [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) to provide IAM identity\-based policies that you use to specify allowed or denied actions and resources and the conditions under which actions are allowed or denied\.

By default, an IAM principal with [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) permissions on a multi\-model endpoint can invoke any model at the address of the S3 prefix defined in the [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) operation, provided that the IAM Execution Role defined in operation has permissions to download the model\. If you need to restrict [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) access to a limited set of models in S3, you can do one of the following:
+ Restrict `InvokeEndpont` calls to specific models hosted at the endpoint by using the `sagemaker:TargetModel` IAM condition key\. For example, the following policy allows `InvokeEndpont` requests only when the value of the `TargetModel` field matches one of the specified regular expressions:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Action": [
                  "sagemaker:InvokeEndpoint"
              ],
              "Effect": "Allow",
              "Resource":
              "arn:aws:sagemaker:region:account-id:endpoint/endpoint_name",
              "Condition": {
                  // TargetModel provided must be from this set of values
                  "StringLike": {
                      "sagemaker:TargetModel": ["company_a/*", "common/*"]
                  }
              }
          },
      ]
  }
  ```

  For information about SageMaker condition keys, see [Condition Keys for Amazon SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonsagemaker.html#amazonsagemaker-policy-keys) in the *AWS Identity and Access Management User Guide*\.
+ Create multi\-model endpoints with more restrictive S3 prefixes\. 

For more information about how SageMaker uses roles to manage access to endpoints and perform operations on your behalf, see [SageMaker Roles ](sagemaker-roles.md)\. Your customers might also have certain data isolation requirements dictated by their own compliance requirements that can be satisfied using IAM identities\.