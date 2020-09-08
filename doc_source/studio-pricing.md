# Amazon SageMaker Studio Pricing<a name="studio-pricing"></a>

When the first member of your team onboards to Amazon SageMaker Studio, Amazon SageMaker creates an Amazon Elastic File System \(Amazon EFS\) volume for the team\. In the SageMaker Studio Control Panel, when the Studio **Status** displays as `Ready`, the Amazon EFS volume has been created\.

When this member, or any member of the team, opens Studio, a home directory is created in the volume for the member\. A storage charge is incurred for this directory\. Subsequently, additional storage charges are incurred for the notebooks and data files stored in the member's home directory\. For pricing information on Amazon EFS, see [Amazon EFS Pricing](http://aws.amazon.com/efs/pricing/)\.

Additional costs are incurred when other operations are run inside Studio, for example, creating an Amazon SageMaker Autopilot job, running a notebook, running training jobs, and hosting a model\.

For information on the costs associated with using Studio notebooks, see [Usage Metering](notebooks-usage-metering.md)\.

For more information about billing along with pricing examples, see [SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\. On the pricing page, look under one of the AWS Regions supported by SageMaker Studio\. For the regions supported by Studio, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.