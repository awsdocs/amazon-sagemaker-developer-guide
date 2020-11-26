# 3D Point Cloud Object Detection<a name="sms-point-cloud-worker-instructions-object-detection"></a>

Use this page to become familiarize with the user interface and tools available to complete your 3D point cloud object detection task\.

**Topics**
+ [Your Task](#sms-point-cloud-worker-instructions-od-task)
+ [Navigate the UI](#sms-point-cloud-worker-instructions-worker-ui-od)
+ [Icon Guide](#sms-point-cloud-worker-instructions-od-icons)
+ [Shortcuts](#sms-point-cloud-worker-instructions-od-hot-keys)
+ [Saving Your Work and Submitting](#sms-point-cloud-worker-instructions-saving-work-od)

## Your Task<a name="sms-point-cloud-worker-instructions-od-task"></a>

When you work on a 3D point cloud object detection task, you need to select a category from the **Annotations** menu on the right side of your worker portal using the **Label Categories** menu\. After you've chosen a category, use the add cuboid and fit cuboid tools to fit a cuboid around objects in the 3D point cloud that this category applies to\. After you place a cuboid, you can modify its dimensions, location, and orientation directly in the point cloud, and the three panels shown on the right\. 

If you see one or more images in your worker portal, you can also modify cuboids in the images or in the 3D point cloud and the edits will show up in the other medium\. 

If you see cuboids have already been added to the 3D point cloud when you open your task, adjust those cuboids and add additional cuboids as needed\. 

To edit a cuboid, including moving, re\-orienting, and changing cuboid dimensions, you must use shortcut keys\. You can see a full list of shortcut keys in the **Shortcuts** menu in your UI\. The following are important key\-combinations that you should become familiar with before starting your labeling task\. 

To edit a cuboid, including moving, re\-orienting, and changing cuboid dimensions, you must use shortcut keys\. You can see a full list of shortcut keys in the **Shortcuts** menu in your UI\. The following are important key\-combinations that you should become familiar with before starting your labeling task\. 


****  

| Mac Command | Windows Command | Action | 
| --- | --- | --- | 
|  Cmd \+ Drag  |  Ctrl \+ Drag  |  Modify the dimensions of the cuboid\. | 
|  Option \+ Drag  |  Alt \+ Drag  |   Move the cuboid\.   | 
|  Shift \+ Drag  |  Shift \+ Drag  |  Rotate the cuboid\.   | 
|  Option \+ O  |  Alt \+ O  |  Fit the cuboid tightly around the points it has been drawn around\. Before using the option, make sure the cuboid fully\-surrounds the object of interest\.   | 
|  Option \+ G  |  Alt \+ G  |  Set the cuboid to the ground\.   | 

## Navigate the UI<a name="sms-point-cloud-worker-instructions-worker-ui-od"></a>

You can navigate in the 3D scene using your keyboard and mouse\. You can:
+ Double click on specific objects in the point cloud to zoom into them\.
+ Use a mouse\-scroller or trackpad to zoom in and out of the point cloud\.
+ Use both keyboard arrow keys and Q, E, A, and D keys to move Up, Down, Left, Right\. Use keyboard keys W and S to zoom in and out\. 

Once you place a cuboids in the 3D scene, a side\-view will appear with three projected views: top, side, and back\. These side\-views show points in and around the placed cuboid and help workers refine cuboid boundaries in that area\. Workers can zoom in and out of each of those side\-views using their mouse\. 

The following video demonstrates movements around the 3D point cloud and in the side\-view\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/object_detection/navigate_od_worker_ui.gif)

When you are in the worker UI, you see the following menus:
+ **Instructions** – Review these instructions before starting your task\.
+ **Shortcuts** – Use this menu to view keyboard shortcuts that you can use to navigate the point cloud and use the annotation tools provided\. 
+ **Label** – Use this menu to modify a cuboid\. First, select a cuboid, and then choose an option from this menu\. This menu includes assistive labeling tools like setting a cuboid to the ground and automatically fitting the cuboid to the object's boundaries\. 
+ **View** – Use this menu to toggle different view options on and off\. For example, you can use this menu to add a ground mesh to the point cloud, and to choose the projection of the point cloud\. 
+ **3D Point Cloud** – Use this menu to add additional attributes to the points in the point cloud, such as color, and pixel intensity\. Note that these options may not be available\.

When you open a task, the move scene icon is on, and you can move around the point cloud using your mouse and the navigation buttons in the point cloud area of the screen\. To return to the original view you see when you first opened the task, choose the reset scene icon\. Resetting the view will not modify your annotations\. 

After you select the add cuboid icon, you can add cuboids to the 3D point cloud visualization\. Once you've added a cuboid, you can adjust it in the three views \(top, side, and front\) and in the images \(if included\)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/object_detection/ot_basic_tools.gif)

You must choose the move scene icon again to move to another area in the 3D point cloud or image\. 

To collapse all panels on the right and make the 3D point cloud full\-screen, choose the full screen icon\. 

If camera images are included, you may have the following view options:
+ **C** – View the camera angle on point cloud view\.
+ **F** – View the frustum, or field of view, of the camera used to capture that image on point cloud view\. 
+ **P** – View the point cloud overlaid on the image\.
+ **B** – View cuboids in the image\. 

The following video demonstrates how to use these view options\. The **F** option is used to view the field of view of the camera \(the gray area\), the **C** options shows the direction the camera is facing and angle of the camera \(blue lines\), and the **B** option is used to view the cuboid\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/view-options-side.gif)

## Icon Guide<a name="sms-point-cloud-worker-instructions-od-icons"></a>

Use this table to learn about the icons you see in your worker task portal\. 


| Icon |  | Description | 
| --- | --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/add_cuobid.png)  |  add cuboid  |  Choose this icon to add a cuboid\. Each cuboid you add is associated with the category you chose\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/edit_cuboid.png)  |  edit cuboid  |  Choose this icon to edit a cuboid\. After you have added a cuboid, you can edit its dimensions, location, and orientation\. After a cuboid is added, it automatically switches to edit cuboid mode\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/fit_scene.png)  |  reset scene  |  Choose this icon to reset the view of the point cloud, side panels, and if applicable, all images to their original position when the task was first opened\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/move_scene.png)  |  move scene  |  Choose this icon to move the scene\. By default, this icon is chosen when you first start a task\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/fullscreen.png)  |  full screen   |  Choose this icon to make the 3D point cloud visualization full screen, and to collapse all side panels\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/label-icons/show.png)  | show labels |  Show labels in the 3D point cloud visualization, and if applicable, in images\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/label-icons/hide.png)  | hide labels |  Hide labels in the 3D point cloud visualization, and if applicable, in images\.   | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/icons/label-icons/delete.png)  | hide labels |  Delete a label\.   | 

## Shortcuts<a name="sms-point-cloud-worker-instructions-od-hot-keys"></a>

The shortcuts listed in the **Shortcuts** menu can help you navigate the 3D point cloud and use tools to add and edit cuboids\. 

Before you start your task, it is recommended that you review the **Shortcuts** menu and become acquainted with these commands\. You need to use some of the 3D cuboid controls to edit your cuboid\. 

## Saving Your Work and Submitting<a name="sms-point-cloud-worker-instructions-saving-work-od"></a>

You should periodically save your work\. Ground Truth will automatically save your work ever 15 minutes\. 

When you open a task, you must complete your work on it before pressing **Submit**\. If you select **Stop Working** you will loose that task, and other workers will be able to start working on it\. 