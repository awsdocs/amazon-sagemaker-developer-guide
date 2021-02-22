# 3D Point Cloud Object Tracking<a name="sms-point-cloud-worker-instructions-object-tracking"></a>

Use this page to become familiarize with the user interface and tools available to complete your 3D point cloud object detection task\.

**Topics**
+ [Your Task](#sms-point-cloud-worker-instructions-ot-task)
+ [Navigate the UI](#sms-point-cloud-worker-instructions-worker-ui-ot)
+ [Bulk Edit Label Category and Frame Attributes](#sms-point-cloud-worker-instructions-ot-bulk-edit)
+ [Icon Guide](#sms-point-cloud-worker-instructions-ot-icons)
+ [Shortcuts](#sms-point-cloud-worker-instructions-ot-hot-keys)
+ [Release, Stop and Resume, and Decline Tasks](#sms-point-cloud-worker-instructions-skip-reject-ot)
+ [Saving Your Work and Submitting](#sms-point-cloud-worker-instructions-saving-work-ot)

## Your Task<a name="sms-point-cloud-worker-instructions-ot-task"></a>

When you work on a 3D point cloud object tracking task, you need to select a category from the **Annotations** menu on the right side of your worker portal using the **Label Categories** menu\. After you've selected a category, use the add cuboid and fit cuboid tools to fit a cuboid around objects in the 3D point cloud that this category applies to\. After you place a cuboid, you can modify its location, dimensions, and orientation directly in the point cloud, and the three panels shown on the right\. If you see one or more images in your worker portal, you can also modify cuboids in the images or in the 3D point cloud and the edits will show up in the other medium\. 

**Important**  
If you see cuboids have already been added to the 3D point cloud frames when you open your task, adjust those cuboids and add additional cuboids as needed\. 

To edit a cuboid, including moving, re\-orienting, and changing cuboid dimensions, you must use shortcut keys\. You can see a full list of shortcut keys in the **Shortcuts** menu in your UI\. The following are important key\-combinations that you should become familiar with before starting your labeling task\. 


****  

| Mac Command | Windows Command | Action | 
| --- | --- | --- | 
|  Cmd \+ Drag  |  Ctrl \+ Drag  |  Modify the dimensions of the cuboid\. | 
|  Option \+ Drag  |  Alt \+ Drag  |   Move the cuboid\.   | 
|  Shift \+ Drag  |  Shift \+ Drag  |  Rotate the cuboid\.   | 
|  Option \+ O  |  Alt \+ O  |  Fit the cuboid tightly around the points it has been drawn around\. Before using the option, make sure the cuboid fully\-surrounds the object of interest\.   | 
|  Option \+ G  |  Alt \+ G  |  Set the cuboid to the ground\.   | 

When you open your task, two frames will be loaded\. If your task includes more than two frames, you need to use the navigation bar in the lower\-left corner, or the load frames icon to load additional frames\. You should annotate and adjust labels in all frames before submitting\. 

After you fit a cuboid tightly around the boundaries of an object, navigate to another frame using the navigation bar in the lower\-right corner of the UI\. If that same object has moved to a new location, add another cuboid and fit it tightly around the boundaries of the object\. Each time you manually add a cuboid, you see the frame sequence bar in the lower\-left corner of the screen turn red where that frame is located temporally in the sequence\.

The following video shows how, if you add a cuboid in one frame, and then adjust it in another, your UI will automatically infer the location of the cuboid in all of the frames in\-between\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/object_tracking/label-interpolation.gif)

Your UI automatically infers the location of that object in all other frames after you've placed a cuboid\. This is called *interpolation*\. You can see the movement of that object, and the inferred and manually created cuboids using the arrows\. Adjust inferred cuboids as needed\. The following video demonstrates how to navigate between frames\. 

**Tip**  
You can turn off the automatic cuboid interpolation across frames using the 3D Point Cloud menu item\. Select **3D Point Cloud** from the top\-menu, and then select **Interpolate Cuboids Across Frames**\. This will uncheck this option and stop cuboid interpolation\. You can reselect this item to turn cuboid interpolation back on\.   
Turning cuboid interpolation off will not impact cuboids that have already been interpolated across frames\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/object_tracking/nav_frames.gif)

Individual labels may have one or more label attributes\. If a label has a label attribute associated with it, it will appear when you select the downward pointing arrow next to the label from the **Label Id** menu\. Fill in required values for all label attributes\. 

You may see frame attributes under the **Label Id** menu\. These attributes will appear on each frame in your task\. Use these attribute prompts to enter additional information about each frame\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/frame-attributes.png)

## Navigate the UI<a name="sms-point-cloud-worker-instructions-worker-ui-ot"></a>

You can navigate in the 3D scene using your keyboard and mouse\. You can:
+ Double click on specific objects in the point cloud to zoom into them\.
+ You can use the \[ and \] keys on your keyboard to zoom into and move from one label to the next\. If no label is selected, when you select \[ or \], the UI will zoom into the first label in the **Label Id** list\. 
+ Use a mouse\-scroller or trackpad to zoom in and out of the point cloud\.
+ Use both keyboard arrow keys and Q, E, A, and D keys to move Up, Down, Left, Right\. Use keyboard keys W and S to zoom in and out\. 

Once you place a cuboids in the 3D scene, a side\-view will appear with three projected views: top, side, and back\. These side\-views show points in and around the placed cuboid and help workers refine cuboid boundaries in that area\. Workers can zoom in and out of each of those side\-views using their mouse\. 

The following video demonstrates movements around the 3D point cloud and in the side\-view\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/object_tracking/nav_frames.gif)

When you are in the worker UI, you see the following menus:
+ **Instructions** – Review these instructions before starting your task\.
+ **Shortcuts** – Use this menu to view keyboard shortcuts that you can use to navigate the point cloud and use the annotation tools provided\. 
+ **Label** – Use this menu to modify a cuboid\. First, select a cuboid, and then choose an option from this menu\. This menu includes assistive labeling tools like setting a cuboid to the ground and automatically fitting the cuboid to the object's boundaries\. 
+ **View** – Use this menu to toggle different view options on and off\. For example, you can use this menu to add a ground mesh to the point cloud, and to choose the projection of the point cloud\.
+ **3D Point Cloud** – Use this menu to add additional attributes to the points in the point cloud, such as color, and pixel intensity\. Note that these options may not be available\.

When you open a task, the move scene icon is on, and you can move around the point cloud using your mouse and the navigation buttons in the point cloud area of the screen\. To return to the original view you see when you first opened the task, choose the reset scene icon\. 

After you select the add cuboid icon, you can add cuboids to the point cloud and images \(if included\)\. You must select the move scene icon again to move to another area in the 3D point cloud or image\. 

To collapse all panels on the right and make the 3D point cloud full\-screen, choose the full screen icon\. 

If camera images are included, you may have the following view options:
+ **C** – View the camera angle on point cloud view\.
+ **F** – View the frustum, or field of view, of the camera used to capture that image on point cloud view\. 
+ **P** – View the point cloud overlaid on the image\.
+ **B** – View cuboids in the image\. 

The following video demonstrates how to use these view options\. The **F** option is used to view the field of view of the camera \(the gray area\), the **C** options shows the direction the camera is facing and angle of the camera \(blue lines\), and the **B** option is used to view the cuboid\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/view-options-side.gif)

### Delete Cuboids<a name="sms-point-cloud-instructions-ot-delete"></a>

You can delete all cuboids \(both manually placed and interpolated\) before and after specific frame using the **Label** top\-menu item\. You may want to use this option if an object leaves a scene, or moves in an abrupt way\. To delete all cuboids before or after the frame you are currently on, select the **Label** menu item and then select one of **Delete in previous frames** or **Delete in next frames**\. Use the **Shortcuts** menu to see the shortcut keys you can use for these options\.

To delete a label in all frames, select **Delete in all frames** from the **Labels** menu, or use the shortcut **Shift \+ Delete** on your keyboard\.

If you try to delete a manually placed cuboid by selecting the cuboid and using the delete icon or the delete key on your keyboard and cuboid interpolation was on when you placed that cuboid one of the following will happen:
+ If the cuboid you delete is the *only* manually placed cuboid created for that label across all frames in your task, the manually placed cuboid is deleted and all interpolated cuboids for that label are deleted from all frames\. For example if you have manually created a single cuboid for Car:1, when you delete the manually placed cuboid all interpolated cuboids with the label Car:1 are removed from the other frames in the task\.
+ If you have manually placed more than one cuboid with the same label in different frames, when you delete one of the manually placed cuboids it is converted to an interpolated cuboid\. Then, all interpolated cuboids adjust\. This adjustment happens because the UI uses manually placed cuboids as anchor points when calculating the location of interpolated cuboid\. When you remove one of these anchor points, the UI must recalcuate the position of interpolated cuboids\. For example, if you have manually placed a cuboid labeled Car:1 in multiple frames, when you delete one of those cuboids it becomes an interpolated cuboid and all interpolated cuboids with the label Car:1 adjust\.

**Note**  
You cannot delete interpolated cuboids using the delete icon or the delete key on your keyboard\. To remove interpolated cuboids from frames, either delete all manually placed cuboids with the same label, or use one of the delete options in the **Label** menu\.



## Bulk Edit Label Category and Frame Attributes<a name="sms-point-cloud-worker-instructions-ot-bulk-edit"></a>

You can bulk edit label attributes and frame attributes\. 

When you bulk edit an attribute, you specify one or more ranges of frames that you want to apply the edit to\. The attribute you select is edited in all frames in that range, including the start and end frames you specify\. When you bulk edit label attributes, the range you specify *must* contain the label that the label attribute is attached to\. If you specify frames that do not contain this label, you will receive an error\.

To bulk edit an attribute you *must* specify the desired value for the attribute first\. For example, if you want to change an attribute from *Yes* to *No*, you must select *No*, and then perform the bulk edit\. 

You can also specify a new value for an attribute that has not been filled in and then use the bulk edit feature to fill in that value in multiple frames\. To do this, select the desired value for the attribute and complete the following procedure\. 

**To bulk edit a label or attribute:**

1. Use your mouse to right click the attribute you want to bulk edit\.

1. Specify the range of frames you want to apply the bulk edit to using a dash \(`-`\) in the text box\. For example, if you want to apply the edit to frames one through ten, enter `1-10`\. If you want to apply the edit to frames two to five, eight to ten and twenty enter `2-5,8-10,20`\.

1. Select **Confirm**\.

If you get an error message, verify that you entered a valid range and that the label associated with the label attribute you are editing \(if applicable\) exists in all frames specified\.

You can quickly add a label to all previous or subsequent frames using the **Duplicate to previous frames** and **Duplicate to next frames** options in the **Label** menu at the top of your screen\. 

## Icon Guide<a name="sms-point-cloud-worker-instructions-ot-icons"></a>

Use this table to learn about the icons you see in your worker task portal\. 


| Icon |  | Description | 
| --- | --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/add_cuobid.png)  |  add cuboid  |  Choose this icon to add a cuboid\. Each cuboid you add is associated with the category you chose\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/edit_cuboid.png)  |  edit cuboid  |  Choose this icon to edit a cuboid\. After you add a cuboid, you can edit its dimensions, location, and orientation\. After a cuboid is added, it automatically switches to edit cuboid mode\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/fit_scene.png)  |  reset scene  | Choose this icon to reset the view of the point cloud, side panels, and if applicable, all images to their original position when the task was first opened\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/move_scene.png)  |  move scene  |  Choose this icon to move the scene\. By default, this icon is chosen when you first start a task\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/fullscreen.png)  |  full screen   |  Choose this icon to make the 3D point cloud visualization full screen and to collapse all side panels\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/load_screen.png)  |  load frames  |  Choose this icon to load additional frames\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/label-icons/hide.png)  | hide labels |  Hide labels in the 3D point cloud visualization, and if applicable, in images\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/label-icons/show.png)  | show labels |  Show labels in the 3D point cloud visualization, and if applicable, in images\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/label-icons/delete.png)  | delete labels |  Delete a label\. This option can only be used to delete labels you have manually created or adjusted\.   | 

## Shortcuts<a name="sms-point-cloud-worker-instructions-ot-hot-keys"></a>

The shortcuts listed in the **Shortcuts** menu can help you navigate the 3D point cloud and use tools to add and edit cuboids\. 

Before you start your task, it is recommended that you review the **Shortcuts** menu and become acquainted with these commands\. You need to use some of the 3D cuboid controls to edit your cuboid\. 

## Release, Stop and Resume, and Decline Tasks<a name="sms-point-cloud-worker-instructions-skip-reject-ot"></a>

When you open the labeling task, three buttons on the top right allow you to decline the task \(**Decline task**\), release it \(**Release task**\), and stop and resume it at a later time \(**Stop and resume later**\)\. The following list describes what happens when you select one of these options:
+ **Decline task**: You should only decline a task if something is wrong with the task, such as an issue with the 3D point clouds, images or the UI\. If you decline a task, you will not be able to return to the task\.
+ **Release Task**: Use this option to release a task and allow others to work on it\. When you release a task, you loose all work done on that task and other workers on your team can pick it up\. If enough workers pick up the task, you may not be able to return to it\. When you select this button and then select **Confirm**, you are returned to the worker portal\. If the task is still available, its status will be **Available**\. If other workers pick it up, it will disappear from your portal\. 
+ **Stop and resume later**: You can use the **Stop and resume later** button to stop working and return to the task at a later time\. You should use the **Save** button to save your work before you select **Stop and resume later**\. When you select this button and then select **Confirm**, you are returned to the worker portal, and the task status is **Stopped**\. You can select the same task to resume work on it\.

  Be aware that the person that creates your labeling tasks specifies a time limit in which all tasks much be completed by\. If you do not return to and complete this task within that time limit, it will expire and your work will not be submitted\. Contact your administrator for more information\. 

## Saving Your Work and Submitting<a name="sms-point-cloud-worker-instructions-saving-work-ot"></a>

You should periodically save your work\. Ground Truth will automatically save your work ever 15 minutes\. 

When you open a task, you must complete your work on it before pressing **Submit**\. 