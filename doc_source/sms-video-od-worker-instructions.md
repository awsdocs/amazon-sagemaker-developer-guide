# Work on Video Frame Object Detection Tasks<a name="sms-video-od-worker-instructions"></a>

Video frame object detection tasks required you to classify and identify the location of objects in video frames using annotations\. A video frame is a still image from a video scene\. 

You can use the worker UI to navigate between video frames and create annotations to identify objects of interest\. Use the sections on this page to learn how to navigate your worker UI, use the tools provided, and complete your task\. 

It is recommended that you complete your task using a Google Chrome web browser\. 

**Important**  
If you see annotations have already been added to one or more video frames when you open your task, adjust those annotations and add additional annotations as needed\. 

**Topics**
+ [Your Task](#sms-video-worker-instructions-od-task)
+ [Navigate the UI](#sms-video-worker-instructions-worker-ui-od)
+ [Tool Guide](#sms-video-worker-instructions-worker-tool-guide-od)
+ [UI Icon Guide](#sms-video-worker-instructions-od-icons)
+ [Shortcuts](#sms-video-worker-instructions-od-hot-keys)
+ [Saving Your Work and Submitting](#sms-video-worker-instructions-saving-work-od)

## Your Task<a name="sms-video-worker-instructions-od-task"></a>

When you work on a video frame object detection task, you need to select a category from the **Label category** menu on the right side of your worker portal to start annotating\. After you've chosen a category, draw annotations around objects that this category applies to\. To learn more about the tools you see in your worker UI, refer to the [Tool Guide](#sms-video-worker-instructions-worker-tool-guide-od)\.

After you've added a label, you may see a downward pointing arrow next to the label in the **Labels** menu\. Select this arrow and then select one option for each label attribute you see to provide more information about that label\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/od_video_attributes.gif)

You may see frame attributes under the **Labels** menu\. These attributes will appear on each frame in your task\. Use these attribute prompts to enter additional information about each frame\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/frame-attributes.png)

To edit an annotation, select the label of the annotation that you want to edit in the **Labels** menu or select the annotation in the frame\. When you edit or delete an annotation, the action will only modify the annotation in a single frame\. 

If you are working on a task that includes a bounding box tool, use the predict next icon to predict the location of all bounding boxes that you have drawn in a frame in the next frame\. If you select a single box and then select the predict next icon, only that box will be predicted in the next frame\. If you have not added any boxes to the current frame, you will receive an error\. You must add at least one box to the frame before using this feature\. 

**Note**  
The predict next feature will not overwrite manually created annotations\. It will only add annotations\. If you use predict next and as a result have more than one bounding box around a single object, delete all but one box\. Each object should only be identified with a single box\. 

After you've used the predict next icon, review the location of each box in the next frame and make adjustments to the box location and size if necessary\. 

The following graphic demonstrates how to use the predict next tool:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/od_predict_next.gif)

For all other tools, you can use the **Copy to next** and **Copy to all** tools to copy your annotations to the next or all frames respectively\. 

## Navigate the UI<a name="sms-video-worker-instructions-worker-ui-od"></a>

You can navigate between video frames using the navigation bar in the bottom\-left corner of your UI\. 

Use the play button to automatically play through multiple frames\. 

Use the next frame and previous frame buttons to move forward or back one frame at a time\. You can also input a frame number to navigate to that frame\. 

The following video demonstrates how to navigate between video frames\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/nav_video_ui_od.gif)

You can zoom in to and out of all video frames\. Once you have zoomed into a video frame, you can move around in that frame using the move icon\. When you navigate to a new view in a single video frame by zooming and moving within that frame, all video frames are set to the same view\. You can reset all video frames to their original view using the fit screen icon\. To learn more, see [UI Icon Guide](#sms-video-worker-instructions-od-icons)\. 

When you are in the worker UI, you see the following menus:
+ **Instructions** – Review these instructions before starting your task\. Additionally, select **More instructions** and review these instructions\. 
+ **Shortcuts** – Use this menu to view keyboard shortcuts that you can use to navigate video frames and use the annotation tools provided\. 
+ **Help** – Use this option to refer back to this documentation\. 

If you 

## Tool Guide<a name="sms-video-worker-instructions-worker-tool-guide-od"></a>

Your task will include one or more tools\. The tool provided dictates the type of anotations you will create to identify and label objects\. Use the following table to learn more about the tool or tools you may see in your worker UI\. 


****  

| Tool | Icon | Action | Description | 
| --- | --- | --- | --- | 
|  Bounding box  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Bounding%20Box.png)  |  Add a bounding box annotation\.  |  Choose this icon to add a bounding box\. Each bounding box you add is associated with the category you choose from the Label category drop down menu\. Select the bounding box or its associated label to adjust it\.   | 
| Predict next |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/PredictNext.png)  |  Predict bounding boxes in the next frame\.  |  Select a bounding box, and then choose this icon to predict the location of that box in the next frame\. You can select the icon multiple times in a row to automatically detect the location of box in multiple frames\. For example, select this icon 5 times to predict the location of a bounding box in the next 5 frames\.   | 
|  Keypoint  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Keypoint.png)  |  Add a keypoint annotation\.  |  Choose this icon to add a keypoint\. Click on an object the image to place the keypoint at that location\.  Each keypoint you add is associated with the category you choose from the Label category drop down menu\. Select a keypoint or its associated label to adjust it\.   | 
|  Polyline  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/polyline.png)  |  Add a polyline annotation\.  |  Choose this icon to add a polyline\. To add a polyline, continuously click around the object of interest to add new points\. To stop drawing a polyline, select the last point that you placed a second time \(this point will be green\), or press **Enter** on your keyboard\.  Each point added to the polyline is connected to the previous point by a line\. The polyline does not have to be closed \(the start point and end point do not have to be the same\) and there are no restrictions on the angles formed between lines\.  Each polyline you add is associated with the category you choose from the Label category drop down menu\. Select the polyline or its associated label to adjust it\.   | 
|  Polygon  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Polygon.png)  |  Add a polygon annotation\.  |  Choose this icon to add a polygon\. To add a polygon, continuously click around the object of interest to add new points\. To stop drawing the polygon, select the start point \(this point will be green\)\.  A polygon is a closed shape defined by a series of points that you place\. Each point added to the polygon is connected to the previous point by a line and there are no restrictions on the angles formed between lines\. Two lines \(sides\) of the polygon cannot cross\. A line will become red if it violates this condition\. The start and end point must be the same\.  Each polyline you add is associated with the category you choose from the Label category drop down menu\. Select the poly  | 
|  Copy to Next  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/copy_to_next.png)  |  Copy annotations to the next frame\.   |  If one or more annotations are selected in the current frame, those annotations are copied to the next frame\. If no annotations are selected, all anotations in the current frame will be copied to the next frame\.   | 
|  Copy to All  |  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/copy_to_all.png)  |  Copy annotations to all subsequent frames\.  |  If one or more annotations are selected in the current frame, those annotations are copied to all subsequent frames\. If no annotations are selected, all anotations in the current frame will be copied to all subsequent frames\.   | 

## UI Icon Guide<a name="sms-video-worker-instructions-od-icons"></a>

Use this table to learn about the icons you see in your worker task portal\. You can automatically select these icons using the keyboard shortcuts found in the **Shortcuts** menu\. 


| Icon |  | Description | 
| --- | --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Brightness.png)  |  brightness  |  Choose this icon to adjust the brightness of all video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Contrast.png)  |  contrast  |  Choose this icon to adjust the contrast of all video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Zoom-in.png)  |  zoom in  |  Choose this icon to zoom into all of the video frames\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Zoom-out.png)  |  zoom out  |  Choose this icon to zoom out of all of the video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Move.png)  |  move screen  |  After you've zoomed into a video frame, choose this icon to move around in that video frame\. You can move around in the video frame using your mouse by clicking and dragging the frame in the direction you want it to move\. This will change the view in all view frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Fit%20screen.png)  | fit screen |  Reset all video frames to their original position\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Undo.png)  | undo |  Undo an action\. You can use this icon to remove a bounding box that you just added, or to undo an adjustment you made to a bounding box\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Redo.png)  | redo | Redo an action that was undone using the undo icon\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Delete.png)  | delete label | Delete a label\. This will delete the bounding box associated with the label in a single frame\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Show:Hide.png)  | show or hide label | Select this icon to show a label that has been hidden\. If this icon has a slash throguh it, select it to hide the label\.  | 

## Shortcuts<a name="sms-video-worker-instructions-od-hot-keys"></a>

The keyboard shortcuts listed in the **Shortcuts** menu can help you quickly select icons, undo and redo annotations, and use tools to add and edit annotations\. For example, once you add a bounding box, you can use **P** to quickly predict the location of that box in subsequent frames\. 

Before you start your task, it is recommended that you review the **Shortcuts** menu and become acquainted with these commands\. 

## Saving Your Work and Submitting<a name="sms-video-worker-instructions-saving-work-od"></a>

You should periodically save your work\. Ground Truth automatically saves your work every 15 minutes\. 

When you open a task, you must complete your work before pressing **Submit**\. If you select **Stop Working**,you lose that task, and other workers can start working on it\. 