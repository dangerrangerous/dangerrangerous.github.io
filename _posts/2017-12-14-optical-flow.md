---
layout: post
comments: true
title: Optical Flow  + Particles
categories: Tech
---


### Optical Flow

<last train video>
<div style="width:100%;height:0;padding-bottom:88%;position:relative;"><iframe src="https://giphy.com/embed/3o6fJ0pgv84GocoYeI" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/experimental-particles-3o6fJ0pgv84GocoYeI">via GIPHY</a></p>

Briefly put, optical flow is the apparent motion of objects relative to each other and the observer. For our purposes 
### optical flow is a technique used in computer vision applications that tracks pixels from one frame to the next,
 creating motion vectors that can help with object detection and trajectory prediction.

Basically we track a pixel’s movement across the screen and assign it values such as speed and direction so we can use this information. I’m going to give a quick overview of how to use optical flow to give particles these velocity values and create cool visual effects in Houdini!

You will need the software packages [Nuke](https://www.foundry.com/products/nuke/non-commercial) 
 and [Houdini] (https://www.sidefx.com/products/houdini-apprentice/),
  there are free versions available for each so go grab them, I’ll wait.

First you’ll need a video, preferably an image sequence. Choosing a video that will work well is an important step and will determine the outcome of your project, try to avoid too much camera movement and have good contrast between your main subjects and the background. Load this into Nuke my creating a read node by pressing “R”.

![OptiFlow_1](/public/OpticalFlow.tut.1.png){:class="img-responsive"}

We then want to place a vector generator node to extract the motion vectors from the pixels in each image. Increase the Vector Detail to 1 and strength to 1.5. If you mouse above the viewport and select “rgba” and change that to “motion” then change the box to the right from “rgba.alpha” to “forward.u” you can get a preview of what the vector generator is doing. But for our purposes leave these at rgba and rgba.alpha.


Next step is to export as .exr files. Drop a write node by pressing “W” and connect it to the VectorGenerator node. Double-click it and in the channels drop down menu near the top right change it from “rgb” to “all”.  Below that choose where you are going to save your .exr files and give it a name such as “AwesomeOptiFlow.###.exr”, the #’s tell Nuke to replace them with the frame number. Hit the render button and throw on some Dropkick Murphys radio to improve your results.


![OptiFlow_2](/public/Tech/OpticalFlow/OpticalFlow.tut.2.png){:class="img-responsive"}
Alright now we open up Houdini - and things get a little complex. Drop a grid down, scope in, connect an ‘attribute from map’ node to the grid node, and then select the .exr sequence we just created for the texture map. Type in “color” in the Texture Channel.  Scale the grid to match the resolution of the image sequence, such as 19.2 x 10.8. Increase the “resolution” of the grid by upping the rows and columns count, notice how these mimic pixels, 192 x 108 is a decent starting point. Connect another ‘attribute from map’ and select the .exr sequence again for texture map. Type in “forward” into texture channel and “v” into Export Attribute to get the vectors from the image. We will use these vectors to influence our particles. Connect a null node to this ‘attribfrommap’ and rename it “EMITTER_OUT”.


![OptiFlow_3]({{/public/Tech/OpticalFlow/OpticalFlow.tut.3.png}}){:class="img-responsive"}
Scope up to object level and rename the grid_object1 geometry to emitter. Mouse to the Particle Fluids tab up top and select “Source Particle Emitter” from the particle tab up top, select our grid, then hit enter. This should create an AutoDopNetwork. Scope up to object level and then into this AutoDopNetwork and press the “L” key to fix the layout. Select the flipsolver, go to the Volume Limits tab and change the Box Size to better fit the shape of the grid, giving some vertical space for the particles to move.


![OptiFlow_4](/public/Tech/OpticalFlow/OpticalFlow.tut.4.png){:class="img-responsive"}
Above the viewport to the right of the ‘pin’ icon there is an icon that you can click to ‘hide other objects’, find it and select it. Drop an ‘attribwrangle’ node and copy the code to add a channel multiplier and allow modification of the @v attribute we made earlier. To the right of the VEXpression box, click the “create parameter” button and slide the multiplier value up. 


![OptiFlow_5](/public/Tech/OpticalFlow/OpticalFlow.tut.5.png){:class="img-responsive"}
Go back into the AutoDopNetwork and under the Guides tab select the Particles tab and change the Visualization Type to None so we can start to visualize colors. You’ll notice that there are no colors presently, let’s fix that. Head to the emitter object and drop an ‘attribtransfer’ node below the ‘create_surface_volume’ node and connect it this and the ‘attribwrangle’ like so.


![OptiFlow_6](/public/Tech/OpticalFlow/OpticalFlow.tut.6.png){:class="img-responsive"}
Now if we go back into the AutoDopNetwork we can see the particles inheriting the velocity vectors from our original .exr files! For the sake of brevity I will be ending this overview/tutorial here. It’s up to you to play around with settings and modify forces to get desired results.


![OptiFlow_7](/public/Tech/OpticalFlow/OpticalFlow.tut.7.png){:class="img-responsive"}
