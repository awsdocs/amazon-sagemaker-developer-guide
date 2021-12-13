# Open the Project Portal<a name="gtp-project-portal"></a>

Once you have successfully submitted the intake form and created a project team, you can access the Ground Truth Plus project by clicking the **Open project portal** button on the AWS console\.

Each project consists of one or more batches\. A *batch* is a collection of recurring similar data objects \(text, image, video frame, and point cloud\) to be labeled\. The project portal provides you with transparency into the data labeling process\. You can stay updated about the project, review the progress of the datasets across multiple projects, and track and analyze project metrics\. The project portal also allows you to review a subset of the labeled data and provide feedback\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gtb-project-portal.png)

You can use the Ground Truth Plus project portal to track the following details about your project\.

**Project name**: Each project is identified using a unique name\.

**Status**: A Ground Truth Plus project has one of the following status types:

1. **Consultation**: An AWS expert collects all the project requirements\.

1. **Workflow design and setup progress**: An AWS expert is setting up your project\.

1. **Pilot in\-progress**: Object labeling for the project in the pilot stage is currently in progress\.

1. **Pilot complete**: Object labeling is complete and the labeled data is stored in your Amazon S3 bucket\.

1. **Pricing complete**: An AWS expert shares the pricing for the production project with you\.

1. **Contract executed**: The contract is complete\.

1. **Production in\-progress**: Labeling for the project in the production stage is in progress\.

1. **Production complete**: Object labeling is complete and the labeled data is stored in your Amazon S3 bucket\.

1. **Paused**: Project is currently paused at your request\.

**Task type**: Ground Truth Plus lets you label five types of tasks that include text, image, video, audio, and point cloud\.

**Batches**: Total number of batches within a project\.

**Project creation date**: Starting date of a project\.

**Total objects**: Total number of objects to be labeled across all batches\.

**Objects completed**: Number of labeled objects\.

**Remaining objects**: Number of objects left to be labeled\.

**Failed objects**: Number of objects that cannot be labeled due to an issue with the input data\.