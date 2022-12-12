# Domain User Profiles<a name="domain-user-profile"></a>

 A user profile represents a single user within an Amazon SageMaker Domain\. The user profile is the main way to reference a user for the purposes of sharing, reporting, and other user\-oriented features\. This entity is created when a user onboards to the Amazon SageMaker Domain\. A user profile can have \(at most\) a single JupyterServer application outside the context of a shared space\. The user profile's Studio application is directly associated with the user profile and has an isolated Amazon EFS directory, an execution role associated with the user profile, and Kernel Gateway applications\. 

**Topics**
+ [Add and Remove User Profiles](domain-user-profile-add-remove.md)
+ [View User Profiles and User Profile Details](domain-user-profile-view-describe.md)