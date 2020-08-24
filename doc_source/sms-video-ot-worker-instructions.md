# Work on Video Frame Object Tracking Tasks<a name="sms-video-ot-worker-instructions"></a>

Video frame object tracking tasks require you to track the movement of objects across video frames\. A video frame is a still image from a video scene\.

You can use the worker UI to navigate between video frames and draw bounding boxes around objects to track their movement from one from to the next\. Use this page to learn how to navigate your worker UI, use the tools provided, and complete your task\. 

It is recommended that you complete your task using a Google Chrome web browser\. 

**Topics**
+ [Your Task](#sms-video-worker-instructions-ot-task)
+ [Navigate the UI](#sms-video-worker-instructions-worker-ui-ot)
+ [Icon Guide](#sms-video-worker-instructions-ot-icons)
+ [Shortcuts](#sms-video-worker-instructions-ot-hot-keys)
+ [Saving Your Work and Submitting](#sms-video-worker-instructions-saving-work-ot)

## Your Task<a name="sms-video-worker-instructions-ot-task"></a>

When you work on a video frame object tracking task, you need to select a category from the **Label category** menu on the right side of your worker portal to start annotating\. After you've chosen a category, draw a bounding box around an object that the category applies to and adjust the box to fit tightly around the object\. This bounding box will be associated with a unique label ID that should only be used for that object\. Use this same label ID to draw additional bounding boxes around the same object in all of the video frames that is appears\. 

After you've added a label, you can quickly add and edit a label category attribute value by using the downward pointing arrow next to the label in the **Label ID** menu\. If you select the pencil icon next to the label in the **Label ID** menu, the **Edit instance** menu will appear\. You can edit the label ID, label category, and label category attributes using this menu\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/ot_video_attribute.gif)

To edit a box, including moving and changing box dimensions, select the label of that box in the **Label ID** menu or select the box in the frame\. When you edit or delete a box, the action will only modify the box in a single frame\. To delete a box associated with a specific label ID in all frames that it appears, use the keyboard shortcut, **Shift** \+ **Delete**\. 

To change the box dimensions, use your mouse to select one of the dots around that box and drag to stretch or shrink the box\. 

To move the box, select the box in the image and drag the box to the desired location\. 

Use the predict next icon to predict the location of all bounding boxes that you have drawn in a frame in the next frame\. If you select a single box and then select the predict next icon, only that box will be predicted in the next frame\. If you have not added any boxes to the current frame, you will receive an error\. You must add at least one box to the frame before using this feature\. 

**Important**  
Each instance ID can only appear once in each frame\. If you use predict next to predict annotations in a frame that you have already annotated, all annotations assocated with instance IDs in that frame that were used in the previous frame will be overwritten\. For example, if you manually annotate frame 1 and frame 2 with three bounding boxes, car:1, car:2 and car:3, and in then use the predict next in frame 1, all of your manual annotations in frame 2 will be overwritten\. 

After you've used the predict next icon, review the location of each box in the next frame and make adjustments to the box location and size if necessary\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/ot_predict_next.gif)

If you see boxes have already been added to one or more video frames when you open your task, adjust those boxes and add additional boxes as needed\. 

## Navigate the UI<a name="sms-video-worker-instructions-worker-ui-ot"></a>

You can navigate between video frames using the navigation bar in the bottom\-left corner of your UI\. 

Use the play button to automatically move through the entire sequence of frames\. 

Use the next frame and previous frame buttons to move forward or back one frame at a time\. You can also input a frame number to navigate to that frame\. 

The following video demonstrates how to navigate between video frames\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/nav_video_ui.gif)

You can zoom in to and out of all video frames\. Once you have zoomed into a video frame, you can move around in that frame using the move icon\. When you set a new view in a single video frame by zooming and moving within that frame, all video frames are set to the same view\. You can reset all video frames to their original view using the fit screen icon\. For additional view options, see [Icon Guide](#sms-video-worker-instructions-ot-icons)\. 

When you are in the worker UI, you see the following menus:
+ **Instructions** – Review these instructions before starting your task\. Additionally, select **More instructions** and review these instructions\. 
+ **Shortcuts** – Use this menu to view keyboard shortcuts that you can use to navigate video frames and use the tools provided\. 
+ **Help** – Use this option to refer back to this documentation\. 

## Icon Guide<a name="sms-video-worker-instructions-ot-icons"></a>

Use this table to learn about the icons you see in your UI\. You can automatically select some of these icons using the keyboard shortcuts found in the **Shortcuts** menu\. 


| Icon |  | Description | 
| --- | --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Bounding%20Box.png)  |  add bounding box  |  Choose this icon to add a bounding box\. Each bounding box you add is associated with the category you choose from the Label category drop down menu\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/PredictNext.png)  |  predict next  |  Select a bounding box, and then choose this icon to predict the location of that box in the next frame\. You can select the icon multiple times in a row to automatically detect the location of box in multiple frames\. For example, select this icon 5 times to predict the location of a bounding box in the next 5 frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Brightness.png)  |  brightness  |  Choose this icon to adjust the brightness of all video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Contrast.png)  |  contrast  |  Choose this icon to adjust the contrast of all video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Zoom-in.png)  |  zoom in  |  Choose this icon to zoom into all of the video frames\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Zoom-out.png)  |  zoom out  |  Choose this icon to zoom out of all of the video frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Move.png)  |  move screen  |  After you've zoomed into a video frame, choose this icon to move around in that video frame\. You can move around the video frame using your mouse by clicking and dragging the frame in the direction you want it to move\. This will change the view in all view frames\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Fit%20screen.png)  | fit screen |  Reset all video frames to their original position\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Undo.png)  | undo |  Undo an action\. You can use this icon to remove a bounding box that you just added, or to undo an adjustment you made to a bounding box\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Redo.png)  | redo | Redo an action that was undone using the undo icon\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Delete.png)  | delete label | Delete a label\. This will delete the bounding box associated with the label in a single frame\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Show:Hide.png)  | show or hide label | Select this icon to show a label that has been hidden\. If this icon has a slash throguh it, select it to hide the label\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/Edit.png)  | edit label | Select this icon to open the Edit instance menu\. Use this menu to edit a label category, ID, and to add or edit label attributes\.  | 

## Shortcuts<a name="sms-video-worker-instructions-ot-hot-keys"></a>

The keyboard shortcuts listed in the **Shortcuts** menu can help you quickly select icons, undo and redo annotations, and use tools to add and edit bounding boxes\. For example, once you add a bounding box, you can use **P** to quickly predict the location of that box in subsequent frames\. 

Before you start your task, it is recommended that you review the **Shortcuts** menu and become acquainted with these commands\.

## Saving Your Work and Submitting<a name="sms-video-worker-instructions-saving-work-ot"></a>

You should periodically save your work using the **Save** button\. Ground Truth will automatically save your work ever 15 minutes\. 

When you open a task, you must complete your work on it before pressing **Submit**\. If you select **Stop Working** you will loose that task, and other workers will be able to start working on it\. 