# Amazon SageMaker Role Manager<a name="role-manager"></a>

Machine learning \(ML\) administrators striving for least\-privilege permissions with Amazon SageMaker must account for a diversity of industry perspectives, including the unique least\-privilege access needs required for personas such as data scientists, machine learning operation \(MLOps\) engineers, and more\. Use Amazon SageMaker Role Manager to build and manage persona\-based IAM roles for common machine learning needs directly through the Amazon SageMaker console\.

Amazon SageMaker Role Manager provides 3 preconfigured role personas and predefined permissions for 12 common ML activities\. Explore the provided personas and their suggested policies, or create and maintain roles for personas unique to your business needs\. If you require additional customization, specify networking and encryption permissions for [Amazon Virtual Private Cloud](http://aws.amazon.com/vpc/) resources and [AWS Key Management Service](http://aws.amazon.com/kms/) encryption keys in [Step 1\. Enter role information](role-manager-tutorial.md#role-manager-tutorial-enter-role-information) of the Amazon SageMaker Role Manager\.

**Topics**
+ [Using the role manager](role-manager-tutorial.md)
+ [Persona reference](role-manager-personas.md)
+ [ML activity reference](role-manager-ml-activities.md)
+ [Launch Studio](role-manager-launch-notebook.md)
+ [Role Manager FAQs](role-manager-faqs.md)