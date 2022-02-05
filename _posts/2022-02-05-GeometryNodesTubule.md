---
layout: post
title: Blender Geometry Nodes Microtubule (3.1 & 3.0)
---

## Geometry Nodes Microtubule

Blender has recently overhauled its fantastic `Geometry Nodes` system with the release of blender 3.0. As a microtubule enthusiast, I figured it was a perfect chance to make a procedural microtubule generator.

<link>

## Nodes setup at a glance

## Features at a glance

## Download 

The geometry nodes setup and some tubulin models are available as an `asset` file. Blender 3.0 introduced the `asset` system and the `asset browser`, which makes using the node system as easy as drag and drop! You can watch a [quick introduction video](https://www.youtube.com/watch?v=ju-nFfL1euk) to get up to speed with that nice feature.

**Download and Use instructions**

1. Download the microtubule asset file (`.blend`). [from this onedrive link](https://1drv.ms/u/s!Agocf5W6i-Ewtkr4N52cFZC2M36l?e=Axhwwl) and save it to whatever (stable) location you like. Consider making a separate directory where you save all your blender assets for convenience. Best to save it in its own directory.

2. Open a new file in blender, and go to `edit` > `preferences` > `file paths ` > `asset libraries` and add the location you stored the asset file in to the list.

3. Drag in a new `area` (see [the official blender docs for info](https://docs.blender.org/manual/en/latest/interface/window_system/areas.html))

4. Change its type to `Asset Browser`

5. From the `Asset Browser`, select the asset file you downloaded (you will have given it a name in step **2**) 

6. Now just drag and drop into your scene! 

   > To get multiple microtubules, you should duplicate (`shift`+`d`) the first microtubule you added to the scene, **rather than dragging in it again from the asset library**. The latter will create completely 'independent' copies, which is probably not what you want.

> **Step 2** you only need to do once. Whenever you make a new file you can always find the assets via the asset library

![asset_browser](../assets/asset_browser.PNG)


