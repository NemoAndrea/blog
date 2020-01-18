---
layout: post
title: Microtuble parody animation  
---

This post will cover some behind the scences stuff of an animation I rendered in Blender that parodied the ad for the original
microsoft surfacebook. Watch it [here](). 

## Building a microtubule

The most important thing is to set up a reasonably accurate model of a microtubule. For this animation, I used the PDB entry 3EDL for this
project. I have a separate post on how to import this into blender [here](https://nemoandrea.github.io/blog/PDBtoBlender/). In this case,
I aligned the tubulin dimer orientation by eye. A better alternative would be to take a PDB entry with a whole microtubule, and then remove
all but one dimer pair. I then used 2 approaches to build the microtubule:
* 2 pairs of array modifiers
* 1 array modifier and follow spline modifiers per protofilament

An array modifier repeats (i.e. copy-paste) the object at specified offset and rotation etc. One modifier was set up to reproduce the
3-start helix of a 13 protofilament microtubules (3 monomer per rotation), the other simply repeats dimers along the protofilament 
(i.e. elongates the microtubule). This works well for the 'blunt' microtubule scenes.

## Microtubule shading

## Video export and Text

Blender outputs a series of png images, one for each frame. I used Davinci resolve (free software) to import these images and their
text functions and export settings to render the video. I use this software for timelapses, as it automatically
handles import of series of images into clips and handles export for youtube. 
