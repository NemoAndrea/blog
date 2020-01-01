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

## Animating the scene
