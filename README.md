# Creating the Big Head AR Effect
Entry to the 2020 Facebook Developer Circles Community Challenge

<p align="center">
  <img width=50% height=50% src="https://user-images.githubusercontent.com/26247014/97118490-12c4c600-1745-11eb-93f6-3e14b8a7059e.jpg">
</p>

# Isolating the Head with Hair Segmentation and Render Pass 
Hair segmentation is achieved with a hair segmentation mask texture, which you can create in Spark AR Studio. You’ll combine this texture with the camera texture in a material.
Render passes allows you to bypass the default render pipeline to produce multiple rendered layers through a custom render pipeline. While they’re a more advanced feature to work with, render passes can give you greater flexibility in what you’re able to create
In this tutorial we’ll look at an effect that uses render passes get the head shape of the user and hair segmentation to get the hair of the user and make a Big Head effect.
Download the sample content to follow this tutorial. You can adapt any of the assets in this effect to create your own projects.


### Getting started
If you want to build this effect yourself, open the unfinished effect in the sample content folder. So you can get started quickly, we've already:
  1.	Imported the **head occluder**.
  2.	Inserted a **face tracker**.
  3.	Inserted and named 1 **null object**, as children of the face tracker. You'll use these to get the shape of the head using render pass.

## Getting the head shape
For this tutorial we’ll isolate the head shape by using a **face mesh** and **head occluder** 3D objects.

Start by creating a **face mesh** as a child of the face tracker that's already in the scene. To help you keep track, rename it. We chose **faceShape**.
Drag the face mesh to **headShapeHolder**.

In the Inspector, create a new material for **faceShape**. Rename the material **faceShape_mat**.

We'll leave all the value of **faceShape_mat** in the Inspector as **default**.

Then we will add the headOccluder as a child of the face tracker that's already in the scene. 
The head occluder is made using 3D objects. You can download Face Reference Assets [here](https://bit.ly/31J9Lcu).

To add the **headOccluder** to scene, drag them from the Assets panel into the Scene panel:
  1.	Drag **headOccluder** onto **faceShapeHolder** to make it a child of it.
  
Your project will look like this:

![2](https://user-images.githubusercontent.com/26247014/97118404-afd32f00-1744-11eb-962d-4fc34bb441a2.jpg)
 
### Tweaking the Head Occluder
To get the best shape of the head, we need to tweak the scale and position of the head occluder.
Adjust the head occluder’s **Scale** and **Position** to get the best head shape. We used Inspector to work out the best size and position. For:
  1.	**Scale**, set **X** to **1.2**, **Y** to **1.25** and **Z** to **1.1**
  2.	**Position**, leave **X** and **Y** to **0**, and change **Z** to **-.01**

We need to change the head occluders material to make it more consistent with the face mesh. headOccluder comes with its own material named **lambert1**. To change this:
  1.	Select **lambert1**.
  2.	Go to **Shader Type** in the Inspector.
  3.	Change it to **Standard**.
  4.	Under Diffuse, click the **Color**. Change it to **White**.
  
![3](https://user-images.githubusercontent.com/26247014/97118405-b06bc580-1744-11eb-9a20-d9315bdb72b5.jpg)

## Getting the Hair shape
We've used Hair Segmentation to get the hair shape.

### Getting the Hair Segmentation texture
To get the Hair segmentation mask:
  1.	From the Scene, click on the **Camera**.
  2.	In the Inspector, click the **+** next to the right of **Texture Extraction**.
  3.	In the Inspector, click the **+** next to the right of **Segmentation**.
  4.	Select **Hair**. 
  
An alert will show like this: 

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118406-b1045c00-1744-11eb-9b14-1f675c29c60d.jpg">
</p>

Click **Remove Facebook**. Hair Segmentation only works on **Instagram Platform**.

Two textures called **cameraTexture0** and **hairSegmentationMaskTexture0** will be listed in the Assets panel.
Your Assets should look like this:

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118407-b19cf280-1744-11eb-8f36-6f87a4c29bb4.jpg">
</p>

 
### Adding the canvas and rectangle
A rectangle will render the textures and material in the scene.

To add the rectangle:
  1.	In the Scene, click **Add Object**.
  2.	Select **Rectangle** - it’ll automatically be added to the scene as a child of a canvas and rename it to **hairRect**.
  ![6](https://user-images.githubusercontent.com/26247014/97118408-b2358900-1744-11eb-83c7-b5c1f898daa3.jpg)
 
The rectangle should completely cover the device’s screen. To do this, adjust the rectangle’s properties:
  1.	In the Inspector, click the box next to **Width** and click **Fill Width** and the box next to **Height** and click **Fill Heigh**t. This will make the rectangle fill the device screen.
  ![7](https://user-images.githubusercontent.com/26247014/97118409-b2358900-1744-11eb-9ad0-bbab5847bb48.jpg)
  
To create the material you’ll add the textures to:
  1.	Select **hairRect** in the Scene panel.
  2.	In the Inspector, go to **Materials** and click **+**.
  
A new material will be added to the Assets panel. Rename it **hair_mat**.
![8](https://user-images.githubusercontent.com/26247014/97118410-b366b600-1744-11eb-9e03-b9764341fa76.jpg)

### Editing the material.
Add the textures you’ve already created to the material:
  1.	Go to the **Inspector**.
  2.	Click **Shader**, change it to **Flat**.
  3.	Under **Diffuse**, click the dropdown next to **Texture** and select **cameraTexture0**.
  4.	Check the box next to **Alpha**. Tt will open an **Invert** and **Texture** options
  5.	Next to **Texture**, click the dropdown and select **hairSegmentationMaskTexture0**.
  6.	Change the **Blend Mode** to **Associative Alpha**.
  7.	On the **Advance Render Option**, Uncheck the **Use Depth Test**. This will make the texture render above all the time.
You’ll see the video of the camera texture playing in the Simulator:
![9](https://user-images.githubusercontent.com/26247014/97118411-b366b600-1744-11eb-85e4-3296660b3152.jpg)
 

### Setting up the scene for the Render Passes
Before we add the render passes. it’s worth setting up the scene for it first. We need to add another rectangle. To do this:
This rectangle will render the head texture and material in the scene.

To add the rectangle:
  1.	In the Scene, click **Add Object**.
  2.	Select **Rectangle** - it’ll automatically be added to the scene as a child of the **canvas0**  and then rename it to **headRect**.
![10](https://user-images.githubusercontent.com/26247014/97118412-b3ff4c80-1744-11eb-86e8-e03213badf6f.jpg)
 
The rectangle should completely cover the device’s screen like we did on the first rectangle. To do this again, adjust the rectangle’s properties:
  1.	In the Inspector, click the box next to **Width** and click **Fill Width** and the box next to **Height** and click **Fill Height**. This will make the rectangle fill the device screen.
![11](https://user-images.githubusercontent.com/26247014/97118413-b497e300-1744-11eb-9f02-3f0a313be683.jpg)
 
### Creating a material
To create the material you’ll add the textures to:
  1.	Select **headRect** in the Scene panel.
  2.	In the Inspector, go to **Materials** and click **+**.
  3.	Select **Create New Material**.
A new material will be added to the Assets panel. Rename it **head_mat**.

### Editing the material.
Add the textures you’ve already created to the material:
1.	Go to the **Inspector**.
2.	Click **Shader**, change it to **Flat**.
3.	Change the **Blend Mode** to **Associative Alpha**.
4.	On the **Advance Render Option**, Uncheck the **Use Depth Test**. This will make the texture render above all the time.

We need it to render the **headRect** before the **hairRect**. In the Scene, **drag** the **headRect** above **hairRect**.

You’ll see the video of the camera texture playing in the Simulator:
![12](https://user-images.githubusercontent.com/26247014/97118414-b5307980-1744-11eb-8bf3-ef6d13bdb058.jpg)
 
Now we’re all set up for the render pass.


### Isolating head using Renderpass
Overview of the finished effect.
Before we go into detail on how each render pass patch functions to isolate the head, it’s worth looking at the project as a whole. To do this, open the finished effect in the sample content folder. In the Patch Editor, you’ll see this graph:

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118415-b5c91000-1744-11eb-8e2d-2b365fbe7b69.jpg">
</p>
 
There are 2 Scene Render Pass patches at the start:
•	The **first Scene Render Pass** patch renders the scene objects and camera texture.
•	The **second Scene Render Pass** isolates the head and use its alpha to render the head from the camera texture.

The second scene render is connected to a series of patch and patch groups. The first patch is **Swizzle**. We used it to take the **alpha (a)** from the **rgba** of the rendered texture

The next patch asset is labeled **Blur**. It add vertical and horizontal blurring effects to the alpha. Creating a smooth edges on the final texture.

### Adding Render Pass
To add render pass pipeline:
1.	Select **Device** in the Scene panel.
2.	In the Inspector, click **Create** besides the **Default Pipeline**

You’ll see new patches in the Patch Editor:  

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118417-b661a680-1744-11eb-9f0b-73080bfa7b05.jpg">
</p>
 
This patch is the start of the render pipeline. It renders everything under the Device by connecting a patch representing the **Device** to the **Scene Object** input port.
The camera texture is rendered in the background, through a patch representing the camera texture connected to the **Background** input in the **Scene Render Pass** patch.

## Adding new Scene Render Pass and other patches
### Creating the patches
Start by creating the additional patches you’ll need to build the start and end of the graph. 

### The Scene Render Pass Patch
Render passes let you bypass the default render pipeline in Spark AR Studio to produce multiple rendered results in your AR effect. While they’re a more advanced feature to work with, they give greater flexibility in what you’re able to create.
A render pass is a computation that uses a combination of textures and numerical parameters to produce another texture. Connecting the result to another render pass patch adds further complexity.
Use them to render scene objects, textures and visual shaders. They’re particularly useful for applying different visual shader treatments to separate scene elements or to the final camera feed.

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118418-b661a680-1744-11eb-9b9b-69760c79afa6.jpg">
</p>
 
It has 2 input ports:
  •	**Background**— this port inputs Texture and add it as background of the Object renders. If no input background , it will render the **Default Color**.
  •	**Object** — this port renders the object and any of its children
  
To add the patch to your project:
  1.	Right-click in the Patch Editor.
  2.	Select **Scene Render Pass** patch from the menu.


### The Swizzle Patch
This patch take input numbers and output them in a different order. It also works on color and textures. We can output the **Alpha** of any **rgba** texture using this Patch.

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118420-b6fa3d00-1744-11eb-95b3-25e96f977a0f.jpg">
</p>
 
To add the patch to your project:
  1.	Right-click in the Patch Editor.
  2.	Select **Swizzle** patch from the menu.

### The Pack Patch
Pack **numbers** and **vector** values into a different vector. It outputted the signals in the same order that they were inputted to the Pack patch.

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118421-b6fa3d00-1744-11eb-9ddd-98a712f582f9.jpg">
</p>

To add the patch to your project:
  1.	Right-click in the Patch Editor.
  2.	Select **Pack** patch from the menu.

We will pack 2 values, **rgb** and **alpha** values. We will change the type of the pack from **Vector3** to **Vector2**. To do this;

1.	Select the **Pack** patch, click the dropdown in the lower part of the patch.
2.	Change **Vector3** to **Vector2**.

### The Blur Patch Asset
This add vertical and horizontal blurring effects to Texture. The patch asset is included on the demo project.
 
<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118422-b792d380-1744-11eb-9c28-792e7ec6e196.jpg">
</p>
 
You can also import it from the **AR Library**. To do this:
1.	On the bottom of Asset Panel, click the **+Add Asset**.
2.	Select Search **AR Library** on the list.
3.	The **AR Library** Window will open. From here, select the **Patch Assets**.
4.	Select **Blur**.

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118423-b82b6a00-1744-11eb-8370-dab892bc1152.jpg">
</p>
 
It will change to different window. Click **Import Free** to import the **Blur** patch asset.

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118424-b8c40080-1744-11eb-82db-55fe0dc79f03.jpg">
</p>
 
The section of the graph will look like this:

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118425-b8c40080-1744-11eb-9ed3-d87b4f480ebd.jpg">
</p>
 
### Building the Patch Graph
To build the patch graph: 

  1.	In the Scene Panel, drag the **headShapeHolder** to the **Patch Editor**.
  2.	Connect the output of the **headShapeHolder** patch to the scene input in the **Scene Render Pass** patch.
  3.	Click the **Default Color** on the **Scene Render Pass** patch and set the **opacity** to **0%**.

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118426-b95c9700-1744-11eb-9195-2115b0a5b36f.jpg">
</p>
 
  4.	Connect the output of the **Scene Render Pass** patch to the Value input in the **Swizzle** patch.
  5.	Set the **Swizzle** value to **a**.
  6.	Connect the output of the **Swizzle** patch to the Texture input in the **Blur** patch asset.
  7.	Set the **Blur** amount to **1**.
  8.	Connect the output of the **Blur** patch asset to the second input in the **Pack** patch.
  9.	Connect the **rgb** output of the **cameraTexture0** to the first input of the **Pack** Patch 

We will input the output of the Pack to the texture of the head_mat. To do this:
  1.	In Asset Panel, select **head_mat**.
  2.	In Inspector Panel, click the circle next to the right of **Texture**.
  3.	Connect the output of the **Pack** patch to the input of the **Diffuse Texture** patch.

The section of the graph will look like this:

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118427-b9f52d80-1744-11eb-80ef-f628f1255dd9.jpg">
</p>

You can now see the result on the Scene View.

![24](https://user-images.githubusercontent.com/26247014/97118429-b9f52d80-1744-11eb-91fe-4820b432b0a1.jpg)

### Making the head bigger
Now that we had isolated the head and hair, we can now make it bigger. To do this:
  1.	Select **headRect** and **hairRect**.
  2.	In Inspector, below the **Transformation**. Change the **Scale** value to **1.5**.
  
![25](https://user-images.githubusercontent.com/26247014/97118431-ba8dc400-1744-11eb-90c6-200beae9cc12.jpg)

### Final Touches
To remove the noises from the scaled rectangles, we need to hide it when there no face detected. To do this:
1.	Drag the **Camera** from the Scene Panel to the **Patch Editor**.
2.	In the Scene panel, select **canvas0**.
3.	In the Inspector, click the circle next to the right of **Visible**.

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118432-bb265a80-1744-11eb-8201-8151752d4969.jpg">
</p>
 
Now we need to connect the patches. 
Connect the **Front Camera** output of the **Camera** Patch to the visible input of **headHairRender** patch. This will hide the canvas when there’s no face detected.

Here’s the Final graph look.

<p align="center">
  <img src="https://user-images.githubusercontent.com/26247014/97118433-bbbef100-1744-11eb-8f78-c6356f49354b.jpg">
</p>


[sparkdownload]: https://sparkar.facebook.com/ar-studio/download/

 
