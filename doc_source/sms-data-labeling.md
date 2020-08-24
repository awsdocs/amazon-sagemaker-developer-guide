# Enhanced Data Labeling<a name="sms-data-labeling"></a>

Amazon SageMaker Ground Truth manages sending your data objects to workers to be labeled\. Labeling each data object is a *task*\. Workers complete each task until the entire labeling job is complete\. Ground Truth divides the total number of tasks into smaller *batches* that are sent to workers\. A new batch is sent to workers when the previous one is finished\.

Ground Truth provides two features that help improve the accuracy of your data labels and reduce the total cost of labeling your data:
+ *Annotation consolidation* helps to improve the accuracy of your data object labels\. It combines the results of multiple workers' annotation tasks into one high\-fidelity label\.
+ *Automated data labeling* uses machine learning to label portions of your data automatically without having to send them to human workers\.

**Topics**
+ [Batches for Labeling Tasks](sms-batching.md)
+ [Consolidate Annotations](sms-annotation-consolidation.md)
+ [Automate Data Labeling](sms-automated-labeling.md)
+ [Chaining Labeling Jobs](sms-reusing-data.md)