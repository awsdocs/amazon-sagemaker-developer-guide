# Work on Video Frame Object Tracking Tasks<a name="sms-video-ot-worker-instructions"></a>

Video frame object tracking tasks require you to track the movement of objects across video frames\. A video frame is a still image from a video scene\.

You can use the worker UI to navigate between video frames and use the tools provided to identify unique objects and track their movement from one from to the next\. Use this page to learn how to navigate your worker UI, use the tools provided, and complete your task\. 

It is recommended that you complete your task using a Google Chrome or Firefox web browser\. 

**Important**  
If you see annotations have already been added to one or more video frames when you open your task, adjust those annotations and add additional annotations as needed\. 

**Topics**
+ [Your Task](#sms-video-worker-instructions-ot-task)
+ [Navigate the UI](#sms-video-worker-instructions-worker-ui-ot)
+ [Bulk Edit Label and Frame Attributes](#sms-video-frame-worker-instructions-ot-bulk-edit)
+ [Tool Guide](#sms-video-worker-instructions-tool-guide)
+ [Icons Guide](#sms-video-worker-instructions-ot-icons)
+ [Shortcuts](#sms-video-worker-instructions-ot-hot-keys)
+ [Release, Stop and Resume, and Decline Tasks](#sms-video-worker-instructions-skip-reject-ot)
+ [Saving Your Work and Submitting](#sms-video-worker-instructions-saving-work-ot)

## Your Task<a name="sms-video-worker-instructions-ot-task"></a>

When you work on a video frame object tracking task, you need to select a category from the **Label category** menu on the right side of your worker portal to start annotating\. After you've chosen a category, use the tools provided to annotate the objects that the category applies to\. This annotation will be associated with a unique label ID that should only be used for that object\. Use this same label ID to create additional annotations for the same object in all of the video frames that it appears in\. Refer to [Tool Guide](#sms-video-worker-instructions-tool-guide) to learn more about the tools provided\.

After you've added a label, you may see a downward pointing arrow next to the label in the **Labels** menu\. Select this arrow and then select one option for each label attribute you see to provide more information about that label\.

You may see frame attributes under the **Labels** menu\. These attributes will appear on each frame in your task\. Use these attribute prompts to enter additional information about each frame\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/frame-attributes.png)

After you've added a label, you can quickly add and edit a label category attribute value by using the downward pointing arrow next to the label in the **Labels** menu\. If you select the pencil icon next to the label in the **Labels** menu, the **Edit instance** menu will appear\. You can edit the label ID, label category, and label category attributes using this menu\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/ot_video_attribute.gif)

To edit an annotation, select the label of the annotation that you want to edit in the **Labels** menu or select the annotation in the frame\. When you edit or delete an annotation, the action will only modify the annotation in a single frame\. 

If you are working on a task that includes a bounding box tool, use the predict next icon to predict the location of all bounding boxes that you have drawn in a frame in the next frame\. If you select a single box and then select the predict next icon, only that box will be predicted in the next frame\. If you have not added any boxes to the current frame, you will receive an error\. You must add at least one box to the frame before using this feature\. 

After you've used the predict next icon, review the location of each box in the next frame and make adjustments to the box location and size if necessary\. 

The following graphic demonstrates how to use the predict next tool:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/ot_predict_next.gif)

For all other tools, you can use the **Copy to next** and **Copy to all** tools to copy your annotations to the next or all frames respectively\. 

## Navigate the UI<a name="sms-video-worker-instructions-worker-ui-ot"></a>

You can navigate between video frames using the navigation bar in the bottom\-left corner of your UI\. 

Use the play button to automatically move through the entire sequence of frames\. 

Use the next frame and previous frame buttons to move forward or back one frame at a time\. You can also input a frame number to navigate to that frame\. 



The following video demonstrates how to navigate between video frames\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/nav_video_ui.gif)

You can zoom in to and out of all video frames\. Once you have zoomed into a video frame, you can move around in that frame using the move icon\. When you set a new view in a single video frame by zooming and moving within that frame, all video frames are set to the same view\. You can reset all video frames to their original view using the fit screen icon\. For additional view options, see [Icons Guide](#sms-video-worker-instructions-ot-icons)\. 

When you are in the worker UI, you see the following menus:
+ **Instructions** – Review these instructions before starting your task\. Additionally, select **More instructions** and review these instructions\. 
+ **Shortcuts** – Use this menu to view keyboard shortcuts that you can use to navigate video frames and use the tools provided\. 
+ **Help** – Use this option to refer back to this documentation\. 

## Bulk Edit Label and Frame Attributes<a name="sms-video-frame-worker-instructions-ot-bulk-edit"></a>

You can bulk edit label attributes and frame attributes \(attributes\)\. 

When you bulk edit an attribute, you specify one or more ranges of frames that you want to apply the edit to\. The attribute you select is edited in all frames in that range, including the start and end frames you specify\. When you bulk edit label attributes, the range you specify *must* contain the label that the label attribute is attached to\. If you specify frames that do not contain this label, you will receive an error\. 

To bulk edit an attribute you *must* specify the desired value for the attribute first\. For example, if you want to change an attribute from *Yes* to *No*, you must select *No*, and then perform the bulk edit\. 

You can also specify a new value for an attribute that has not been filled in and then use the bulk edit feature to fill in that value in multiple frames\. To do this, select the desired value for the attribute and complete the following procedure\. 

**To bulk edit a label or attribute:**

1. Use your mouse to right click the attribute you want to bulk edit\.

1. Specify the range of frames you want to apply the bulk edit to using a dash \(`-`\) in the text box\. For example, if you want to apply the edit to frames one through ten, enter `1-10`\. If you want to apply the edit to frames two to five, eight to ten and twenty enter `2-5,8-10,20`\.

1. Select **Confirm**\.

If you get an error message, verify that you entered a valid range and that the label associated with the label attribute you are editing \(if applicable\) exists in all frames specified\.

You can quickly add a label to all previous or subsequent frames using the **Duplicate to previous frames** and **Duplicate to next frames** options in the **Label** menu at the top of your screen\. 

## Tool Guide<a name="sms-video-worker-instructions-tool-guide"></a>

Your task will include one or more tools\. The tool provided dictates the type of annotations you will create to identify and track objects\. Use the following table to learn more about each tool provided\. 


****  

| Tool | Icon | Action | Description | 
| --- | --- | --- | --- | 
|  Bounding box  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Bounding%20Box.png)  |  Add a bounding box annotation\.  |  Choose this icon to add a bounding box\. Each bounding box you add is associated with the category you choose from the Label category drop down menu\. Select the bounding box or its associated label to adjust it\.   | 
| Bounding box |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/PredictNext.png)  |  Predict bounding boxes in the next frame\.  |  Select a bounding box, and then choose this icon to predict the location of that box in the next frame\. You can select the icon multiple times in a row to automatically detect the location of box in multiple frames\. For example, select this icon 5 times to predict the location of a bounding box in the next 5 frames\.   | 
|  Keypoint  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Keypoint.png)  |  Add a keypoint annotation\.  |  Choose this icon to add a keypoint\. Click on an object the image to place the keypoint at that location\.  Each keypoint you add is associated with the category you choose from the Label category drop down menu\. Select a keypoint or its associated label to adjust it\.   | 
|  Polyline  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/polyline.png)  |  Add a polyline annotation\.  |  Choose this icon to add a polyline\. To add a polyline, continuously click around the object of interest to add new points\. To stop drawing a polyline, select the last point that you placed a second time \(this point will be green\), or press **Enter** on your keyboard\.  Each point added to the polyline is connected to the previous point by a line\. The polyline does not have to be closed \(the start point and end point do not have to be the same\) and there are no restrictions on the angles formed between lines\.  Each polyline you add is associated with the category you choose from the Label category drop down menu\. Select the polyline or its associated label to adjust it\.   | 
|  Polygon  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Polygon.png)  |  Add a polygon annotation\.  |  Choose this icon to add a polygon\. To add a polygon, continuously click around the object of interest to add new points\. To stop drawing the polygon, select the start point \(this point will be green\)\.  A polygon is a closed shape defined by a series of points that you place\. Each point added to the polygon is connected to the previous point by a line and there are no restrictions on the angles formed between lines\. The start and end point must be the same\.  Each polyline you add is associated with the category you choose from the Label category drop down menu\. Select the polyline or its associated label to adjust it\.   | 
|  Copy to Next  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/copy_to_next.png)  |  Copy annotations to the next frame\.   |  If one or more annotations are selected in the current frame, those annotations are copied to the next frame\. If no annotations are selected, all annotations in the current frame will be copied to the next frame\.   | 
|  Copy to All  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/copy_to_all.png)  |  Copy annotations to all subsequent frames\.  |  If one or more annotations are selected in the current frame, those annotations are copied to all subsequent frames\. If no annotations are selected, all annotations in the current frame will be copied to all subsequent frames\.   | 

## Icons Guide<a name="sms-video-worker-instructions-ot-icons"></a>

Use this table to learn about the icons you see in your UI\. You can automatically select some of these icons using the keyboard shortcuts found in the **Shortcuts** menu\. 


| Icon | Action  | Description | 
| --- | --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Brightness.png)  |  brightness  |  Choose this icon to adjust the brightness of all video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Contrast.png)  |  contrast  |  Choose this icon to adjust the contrast of all video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Zoom-in.png)  |  zoom in  |  Choose this icon to zoom into all of the video frames\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Zoom-out.png)  |  zoom out  |  Choose this icon to zoom out of all of the video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Move.png)  |  move screen  |  After you've zoomed into a video frame, choose this icon to move around in that video frame\. You can move around the video frame using your mouse by clicking and dragging the frame in the direction you want it to move\. This will change the view in all view frames\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Fit%20screen.png)  | fit screen |  Reset all video frames to their original position\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Undo.png)  | undo |  Undo an action\. You can use this icon to remove a bounding box that you just added, or to undo an adjustment you made to a bounding box\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Redo.png)  | redo | Redo an action that was undone using the undo icon\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Delete.png)  | delete label | Delete a label\. This will delete the bounding box associated with the label in a single frame\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Show_Hide.png)  | show or hide label | Select this icon to show a label that has been hidden\. If this icon has a slash through it, select it to hide the label\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Edit.png)  | edit label | Select this icon to open the Edit instance menu\. Use this menu to edit a label category, ID, and to add or edit label attributes\.  | 

## Shortcuts<a name="sms-video-worker-instructions-ot-hot-keys"></a>

The keyboard shortcuts listed in the **Shortcuts** menu can help you quickly select icons, undo and redo annotations, and use tools to add and edit annotations\. For example, once you add a bounding box, you can use **P** to quickly predict the location of that box in subsequent frames\. 

Before you start your task, it is recommended that you review the **Shortcuts** menu and become acquainted with these commands\.

## Release, Stop and Resume, and Decline Tasks<a name="sms-video-worker-instructions-skip-reject-ot"></a>

When you open the labeling task, three buttons on the top right allow you to decline the task \(**Decline task**\), release it \(**Release task**\), and stop and resume it at a later time \(**Stop and resume later**\)\. The following list describes what happens when you select one of these options:
+ **Decline task**: You should only decline a task if something is wrong with the task, such as unclear video frame images or an issue with the UI\. If you decline a task, you will not be able to return to the task\.
+ **Release Task**: Use this option to release a task and allow others to work on it\. When you release a task, you loose all work done on that task and other workers on your team can pick it up\. If enough workers pick up the task, you may not be able to return to it\. When you select this button and then select **Confirm**, you are returned to the worker portal\. If the task is still available, its status will be **Available**\. If other workers pick it up, it will disappear from your portal\. 
+ **Stop and resume later**: You can use the **Stop and resume later** button to stop working and return to the task at a later time\. You should use the **Save** button to save your work before you select **Stop and resume later**\. When you select this button and then select **Confirm**, you are returned to the worker portal, and the task status is **Stopped**\. You can select the same task to resume work on it\. 

  Be aware that the person that creates your labeling tasks specifies a time limit in which all tasks much be completed by\. If you do not return to and complete this task within that time limit, it will expire and your work will not be submitted\. Contact your administrator for more information\. 

## Saving Your Work and Submitting<a name="sms-video-worker-instructions-saving-work-ot"></a>

You should periodically save your work using the **Save** button\. Ground Truth will automatically save your work ever 15 minutes\. 

When you open a task, you must complete your work on it before pressing **Submit**\. 