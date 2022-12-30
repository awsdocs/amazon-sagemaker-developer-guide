# Amazon SageMaker Domain<a name="studio-entity-status"></a>

Amazon SageMaker Domain supports SageMaker machine learning \(ML\) environments\. A SageMaker Domain is composed of the following entities\. For onboarding steps to create a Domain, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.
+  *Domain*: An Amazon SageMaker Domain consists of an associated Amazon Elastic File System \(Amazon EFS\) volume; a list of authorized users; and a variety of security, application, policy, and Amazon Virtual Private Cloud \(Amazon VPC\) configurations\. Users within a Domain can share notebook files and other artifacts with each other\. An account can have multiple Domains\. For more information about multiple Domains, see [Multiple Domains Overview](domain-multiple.md)\.
+  *UserProfile*: A user profile represents a single user within a Domain\. It is the main way to reference a user for the purposes of sharing, reporting, and other user\-oriented features\. This entity is created when a user onboards to the Amazon SageMaker Domain\. For more information about user profiles, see [Domain User Profiles](domain-user-profile.md)\.
+  *shared space*: A shared space consists of a shared JupyterServer application and shared directory\. All users within the Domain have access to the shared space\. All user profiles in a Domain have access to all shared spaces in the Domain\. For more information about shared spaces, see [Collaborate with shared spaces](domain-space.md)\.
+  *App*: An app represents an application that supports the reading and execution experience of the user’s notebooks, terminals, and consoles\. The type of app can be JupyterServer, KernelGateway, RStudioServerPro, or RSession\. A user may have multiple apps active simultaneously\.

The following tables describe the status values for the `Domain`, `UserProfile`, `shared space`, and `App` entities\. Where applicable, they also give troubleshooting steps\.


**Domain status values**  

| Value | Description | 
| --- | --- | 
| Pending | Ongoing creation of Domain\. | 
| InService | Successful creation of Domain\. | 
| Updating | Ongoing update of Domain\. | 
| Deleting | Ongoing deletion of Domain\. | 
| Failed | Unsuccessful creation of Domain\. Call the DescribeDomain API to see the failure reason for Domain creation\. Delete the failed Domain and recreate the Domain after fixing the error mentioned in FailureReason\. | 
| Update\_Failed | Unsuccessful update of Domain\. Call the DescribeDomain API to see the failure reason for Domain update\. Call the UpdateDomain API after fixing the error mentioned in FailureReason\. | 
| Delete\_Failed | Unsuccessful deletion of Domain\. Call the DescribeDomain API to see the failure reason for Domain deletion\. Because deletion failed, you might have some resources that are still running, but you cannot use or update the Domain\. Call the DeleteDomain API again after fixing the error mentioned in FailureReason\. | 


**`UserProfile` status values**  

| Value | Description | 
| --- | --- | 
| Pending | Ongoing creation of UserProfile\. | 
| InService | Successful creation of UserProfile\. | 
| Updating | Ongoing update of UserProfile\. | 
| Deleting | Ongoing deletion of UserProfile\. | 
| Failed | Unsuccessful creation of UserProfile\. Call the DescribeUserProfile API to see the failure reason for UserProfile creation\. Delete the failed UserProfile and recreate it after fixing the error mentioned in FailureReason\. | 
| Update\_Failed | Unsuccessful update of UserProfile\. Call the DescribeUserProfile API to see the failure reason for UserProfile update\. Call the UpdateUserProfile API again after fixing the error mentioned in FailureReason\. | 
| Delete\_Failed | Unsuccessful deletion of UserProfile\. Call the DescribeUserProfile API to see the failure reason for UserProfile deletion\. Because deletion failed, you might have some resources that are still running, but you cannot use or update the UserProfile\. Call the DeleteUserProfile API again after fixing the error mentioned in FailureReason\. | 


**shared space status values**  

| Value | Description | 
| --- | --- | 
| Pending | Ongoing creation of shared space\. | 
| InService | Successful creation of shared space\. | 
| Deleting | Ongoing deletion of shared space\. | 
| Failed | Unsuccessful creation of shared space\. Call the DescribeSpace API to see the failure reason for shared space creation\. Delete the failed shared space and recreate it after fixing the error mentioned in FailureReason\. | 
| Update\_Failed | Unsuccessful update of shared space\. Call the DescribeSpace API to see the failure reason for shared space update\. Call the UpdateSpace API again after fixing the error mentioned in FailureReason\. | 
| Delete\_Failed | Unsuccessful deletion of shared space\. Call the DescribeSpace API to see the failure reason for shared space deletion\. Because deletion failed, you might have some resources that are still running, but you cannot use or update the shared space\. Call the DeleteSpace API again after fixing the error mentioned in FailureReason\. | 
| Deleted | Successful deletion of shared space\. | 


**`App` status values**  

| Value | Description | 
| --- | --- | 
| Pending | Ongoing creation of App\. | 
| InService | Successful creation of App\. | 
| Deleting | Ongoing deletion of App\. | 
| Failed | Unsuccessful creation of App\. Call the DescribeApp API to see the failure reason for App creation\. Call the CreateApp API again after fixing the error mentioned in FailureReason\. | 
| Deleted | Successful deletion of App\. | 

**Topics**
+ [Prerequisites](domain-prerequisites.md)
+ [Multiple Domains Overview](domain-multiple.md)
+ [Domain resource isolation](domain-resource-isolation.md)
+ [Setting Defaults for a Domain](domain-set-defaults.md)
+ [Environment](domain-space-environment.md)
+ [View and Edit Domains](domain-view-edit.md)
+ [Delete an Amazon SageMaker Domain](gs-studio-delete-domain.md)
+ [Domain User Profiles](domain-user-profile.md)
+ [IAM Identity Center Groups in a Domain](domain-groups.md)
+ [Collaborate with shared spaces](domain-space.md)