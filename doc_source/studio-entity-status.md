# Studio Entity Status<a name="studio-entity-status"></a>

Amazon SageMaker Studio creates the following entities for various purposes\.
+  *Domain*: A SageMaker Studio domain consists of an associated Amazon Elastic File System \(Amazon EFS\) volume; a list of authorized users; and a variety of security, application, policy, and Amazon Virtual Private Cloud \(Amazon VPC\) configurations\. An AWS account is limited to one domain per Region\. Users within a domain can share notebook files and other artifacts with each other\.
+  *UserProfile*: A user profile represents a single user within a domain, and is the main way to reference a user for the purposes of sharing, reporting, and other user\-oriented features\. This entity is created when a user onboards to SageMaker Studio\.
+  *App*: An app represents an application that supports the reading and execution experience of the user’s notebooks, terminals, and consoles\. Apps can be either a JupyterServer or a KernelGateway\. This operation is automatically invoked by SageMaker Studio upon access to the associated Domain, and when new kernel configurations are selected by the user\. A user may have multiple Apps active simultaneously\.

The following tables describe the status values for the Domain, UserProfile, and App entities\. Where applicable, they also give troubleshooting steps\.


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


**UserProfile status values**  

| Value | Description | 
| --- | --- | 
| Pending | Ongoing creation of UserProfile\. | 
| InService | Successful creation of UserProfile\. | 
| Updating | Ongoing update of UserProfile\. | 
| Deleting | Ongoing deletion of UserProfile\. | 
| Failed | Unsuccessful creation of UserProfile\. Unsuccessful creation of UserProfile\. Call the DescribeUserProfile API to see the failure reason for UserProfile creation\. Delete the failed UserProfile and recreate it after fixing the error mentioned in FailureReason\. | 
| Update\_Failed | Unsuccessful update of UserProfile\. Call the DescribeUserProfile API to see the failure reason for UserProfile update\. Call the UpdateUserProfile API again after fixing the error mentioned in FailureReason\. | 
| Delete\_Failed | Unsuccessful deletion of UserProfile\. Call the DescribeUserProfile API to see the failure reason for UserProfile deletion\. Because deletion failed, you might have some resources that are still running, but you cannot use or update the UserProfile\. Call the DeleteUserProfile API again after fixing the error mentioned in FailureReason\. | 


**App status values**  

| Value | Description | 
| --- | --- | 
| Pending | Ongoing creation of App\. | 
| InService | Successful creation of App\. | 
| Deleting | Ongoing deletion of App\. | 
| Failed | Unsuccessful creation of App\. Call the DescribeApp API to see the failure reason for App creation\. Call the CreateApp API again after fixing the error mentioned in FailureReason\. | 
| Deleted | Successful deletion of App\. | 