---
layout: post
title: Animating proteins in Blender
---

This article builds on my [previous blog post](https://nemoandrea.github.io/blog/PDBtoBlender/). I will go over how I personally make protein animiations and work with Blender.
Note that I am a novice with blender, so there are definitely things that could be improved. I appreciate any tips and tricks;
please email be and I'll include them.

So you have the structure open in blender, what now? Unless you are rendering a very small scene or have a very powerful device you are working on, I recommend taking a minute to try to minimise the computational impact your model has. While I'm sure there are many good ways to do this, I personally use either of the following two:
* a decimate modifier on the object 
* a remesh modifier on the object

Remesh takes the longest to compute, but seems to be best for very large structures. For the decimate modifier, I would recommend the 'planar' option and just increase the angle to acceptable levels. After you find something you like, right click on the protein surface in the viewport and click 'shade smooth'; this will make for nicer looking interpolation of the vertices. For the remesh modifier, start with a low number (should be by 4 default) and increase 1 at a time until you find the detail to be close enough to the original. Make note of the original vertex count and what the count is after the remesh. You'll want o make sure it's lower after the remesh. For remesh, tick the 'smooth shading' box instead of the method above. Once you think you have reduced the number of vertices enough you can get to the fun part: creating your scene.

## Arranging the scene 

*This section is for people new to blender*

For my work I only use the two tabs highlighted in red in blender. The tabs are just presets for what windows should be open; in principle you can rearrange each of the tabs to look like another.

![alt text](https://lh3.googleusercontent.com/zJJ_CjiSSb3omDqDsUt-cLujw143KjkPZT6U8v-_AC3mrEZKRl6Zw2-td665i95LKmDC8LLnR7BXzn17U05qNMoGQQ12YN5pXPre4scBMkx6bvSlnksDZWGOvou5RukVi7a-_PoJ2T0lsM__GsXmzdy9EF7agAVJIg_EjAcenChP_P6VOv78AXw8Fc6B0bV2FC_DpFXYU3ymT8bx3rtpLCWKBDgTBgR5zcNeyzJM7GV3DHwzeP9mrBI4sHVzL5YcUPVKAs8F-X-6OfuJVzftC0nydtJ4zv4X3yuRFnksc22yvAjBSgjKNggNS56kciUSP8I_VtEpwQSQNg_H0z6oYtpHvqZxvdBs_7nKtHGW-Wvn8t6X2eebq0EJhVxnD2yFTGwEZUiSnQjmk-ikb3VG3BOV0IjkT_3rbsV2o-YOj0_ceqhfvRU_N0If92WUOygY7w7JPNBYB3ivQIC0oNSEaAVx5jjjoZn8yQGRJtdywUpTGbfVeL3Nrh2mA4nDN6Nb5QGn4hqfZM_SjcAr6jgEoHVrOa3EqUHnSeS6w7UjsApM7UG8OUCyOfpQgALIbeM3JObqcgl4pL2Wpi2NBnK4vRYcy9h3dOwo_09L2XLYrY_TlqpBvDEAKDKgiScbXtCGLK5EE12RakwVB8KO72fc1vnoU2WMnSyHXc6ZWKMZgM3n7Kf5Tww2Ab5L=w1858-h972-no "Blender Viewport")


Lets go over the other buttons that would be important to use:
* blue: this toggles overlays. Overlays includes the grid, representations for cameras and lights and outlines for objects. You'll want to turn this off when rendering, but on whenever you are working on your scence
* Orange: shading options. I use two options. The first (nr 2 out of 4) does not show the material you set for your protein (surface appearance), but shades everything a practical gray. Best for performance and arranging items. The Second (nr 4 out of 4) is the shaded view, which shows both surface appearance and the lighting of your scence. This is what will be used for the rendered image too. This will show you exactly what the render will look like.
* purple: this is a little pop out window with basic transformations, I prefer to use this one over others.
* green: this should say object. You can switch to edit mode (and back) if you press 'TAB' but we wont need this. 
* yellow: this is your scene manager, it's basically the overview of all objects (lights, cameras, splines, surfaces etc) and their organisation. 
* dark blue: editor. This holds a long list of tabs with the many things you can do. Some of the tabs are general (e.g. the first tab), some only apply to the selected object, and some options are unique to the type of selected object (e.g. lamp tab is only available to light sources).

For protein structures, I would recommend you reserve the imported structures as templates and only work with copies in the scene (just ctrl-c ctrl-v). It would be wise to first arrange the objects and only then worry about surface appearance and lighting.

Basic navigation: the numberpad will allow for basic viewing angles (x,y,z, camera). pressing and holding down middle mouse will allow for free panning around a rotation point. Double pressing '.' on the *numpad* will centre the rotation point on the selected object.

## Animating the scene

To animate, it is easiest to use blender's keyframes. In fact, using blender's graphical interface and combining with the scripting is smarter than trying to do everything by scripting. Arranging the scene and animating is easier with the blender tools. We time our animation with the help of the animation timeline. This can be accessed by adding a new window to the viewport by sliding up on the bottom left of the 3D viewport in the 'overview' tab (orange in image below). You know you can add a new subwindow once your cursur turns to a crosshair. You can then select 'timeline' for the new subwindow in the dropdown in the topleft (cyan in image below) (or press shift + F12). 


![alt text](https://lh3.googleusercontent.com/acY63747xVAUrNAs-0NV7LIbL-v_G6kzff62SiiSf8oEfGS83GQwmCfK6nmrQYzOPbbw9Wg58cMW1wGsDa8N7KwVVe_XSlLjUIfy6MJL_PcEdgkP9_paKcgfoRXM9Hs6CvDu1DYwxs_Gv4-8_3wceAof-fiYhWc5-L0oOo8so6KUiwy4YjGbDlqRAIengC_UalmT5MyGjUJD7jzzKqDecILJoEmHaTTnOkc-fZ7QIcC3MlP71fbOk-EVkltUnxO5WScFNnoigxwQFe0GwoTa0NfebjB9DzcM6CEv4VjzUbRq6VPzCwz4v4n3EZY6c9wRgERGW_c26key4D8mtj5XS0NxjEIopPlDSLKg3UCp4HVmz4K-6RX5iQO2QRcLIBSVofybJnDJw4OseNLSayom3DL6XJFJGCXXAViPLSq8TpkeJ7_SuvqT_0nXQFJZmfzza-1lJlbnlwfsAaD5cArDRaVdVZ9-1i0VljLFblsDRv9kdjKTpXl-z-qza1uHuEYenAde8YqYRCqkN_D9me3wZI4FFvIvVT4cRq0mRSPK2Gr5nI7lmvcwW7RAY7pLSER6QLJ-x4_o4alhLWkEUsR05qvidetmfMl9-yFd_2dBcTZJVTHodcYzpMYIBa0ou9nR9oC05ib7-Txt_7faNlEPuTPvQ_x8SiuDSUMIrfAPoOfcsBhuFRBc9Ntz=w1565-h977-no "Opening up the timeline subwindow")
You then set up all animation based on keyframes. [This is a good starting point](https://www.versluis.com/2015/04/how-to-create-keyframe-animations-in-blender/). You can then sync these up with your python by setting the current timepoint via python.

## Scripting

You can do python scripting straight from blender. It should also be possible to link to external files, but this was giving me a bit of trouble so I just did everything in the same file. To allow your python script to interface with blender you just have to add 'import bpy' to your imports.

There are two consoles in blender. One is part of a subwindow just like all objects in blender, and one is the 'main' console. Print statements get displayed in the latter, so that one is the most useful. You open it by [window] -> [toggle system console] in the top left of blender. 

