# Delete an Amazon SageMaker Studio Domain<a name="gs-studio-delete-domain"></a>


****  

|  | 
| --- |
| Amazon SageMaker Studio Notebooks is in preview release and is subject to change\. | 

When you onboard to Amazon SageMaker Studio using IAM authentication, Studio creates a domain for your account\. A domain consists of a list of authorized users, configuration settings, and an Amazon Elastic File System \(Amazon EFS\) volume, which contains data for the users, including notebooks, resources, and artifacts\.

To return Studio to the state it was in before you onboarded, you must delete this domain\. This is required if you want to switch authentication modes from IAM to AWS SSO\.

When a domain has been deleted, all members of the domain lose access to the Amazon EFS volume and the data it contains\. The data is not deleted, but you must manually import the notebooks and other resources to reuse them in Studio\.

To delete a domain, the domain cannot contain any user profiles or apps\. To delete a user profile, the profile cannot contain any apps\.

**Note**  
Amazon SageMaker Studio is available only in the US East \(Ohio\) Region\.  
You must have admin permission to delete a domain\.

**To delete a domain**

1. Retrieve the list of applications in the domain\.

   ```
   aws --region us-east-2 sagemaker list-apps \
       --domain-id-equals DomainId
   ```

1. Delete each application in the list\.

   ```
   aws --region us-east-2 sagemaker delete-app \
       --domain-id DomainId \
       --app-name AppName \
       --app-type AppType \
       --user-profile-name UserProfileName
   ```

1. Retrieve the list of user profiles in the domain\.

   ```
   aws --region us-east-2 sagemaker list-user-profiles \
       --domain-id-equals DomainId
   ```

1. Delete each user profile in the list\.

   ```
   aws --region us-east-2 sagemaker delete-user-profile \
       --domain-id DomainId \
       --user-profile-name UserProfileName
   ```

1. Delete the domain\.

   ```
   aws --region us-east-2 sagemaker delete-domain \
       --domain-id DomainId
   ```