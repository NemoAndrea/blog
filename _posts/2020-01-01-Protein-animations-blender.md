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

![alt text](https://lh3.googleusercontent.com/6Sq3aOfIVSgk3hxrJAuoWJVbJ1GSccIh6j34hIw2NFkkVA0YMQsJ_oRv-inAdqjeyxPoz8Q7FaxiG_XvZQkMVpMonZqys0x7uwj-UGMDFngyMfuKl-ATeP__69gg3KCOyeAcJdwK1TKEbaNU3UeI7yuYNFqaUsVA0hBFGUyaeKpdy61oFtc8NTeNATtnxqvSQ1s-wjJlEBpTVSQOSUMKoI8p63Optn4lWa__pdjfaC5p45aNNGzhLbMHpJE48Xqnwg8B-GKoYgNuRNYAzR-yUal_sPjTL1AKTlZl9cqCn0GLFim7WhCT3B-g0v3u3g1eI3aGOiHLcd_DhRYUIT81owKsRsYtV_ht-4G7DO4BPi_mK23p6mzOd1_7fAQSPhLyc7UHeeOgQfXKBtcPY80kNT_hMGWZrq1E2fQS9Hv85psptPH-SCX-JDxmFm1OJUTdfNdfmDZBtzQTaFpvjafuuGN3YJ0xX-MJeq-ihpQ3BPip1WbtXjrPgZXkop6iDzihusySGaqBDrv5uSj0y8I8yiBly13oTOM_sepjrD9yI-RNMRxSUS8unqNHtN7vj0TkcTIuz78uWHDihcmwm93ufsnCg_6iKKiy9mGnNjygQ0N-6QQrL520-qS-079g2PKOQOJ6tw5VjAvtfcdc4nk_LIemZfDeUtwOnMW252Q3TMy0VfnskkFNjAo3c3RSI3hFPBYUShpZ6K7O-a8uaBdh7TcRcfeBRRx_nCdIz_IGNk2f_LAdDw=w927-h528-no "Blender Viewport")


Lets go over the other buttons that would be important to use:
* blue: this toggles overlays. Overlays includes the grid, representations for cameras and lights and outlines for objects. You'll want to turn this off when rendering, but on whenever you are working on your scence
* Orange: shading options. I use two options. The first does not show the material you set for your protein (surface appearance), but shades everything a practical gray. Best for performance and arranging items. The Second is the shaded view, which shows both surface appearance and the lighting of your scence. This is what will be used for the rendered image too. This will show you exactly what the render will look like.
* purple: this is a little pop out window with basic transformations, I prefer to use this one over others.
* green: this should say object. You can switch to edit mode (and back) if you press 'TAB' but we wont need this. 
* yellow: this is your scene manager, it's basically the overview of all objects (lights, cameras, splines, surfaces etc) and their organisation. 
* dark blue: editor. This holds a long list of tabs with the many things you can do. Some of the tabs are general (e.g. the first tab), some only apply to the selected object, and some options are unique to the type of selected object (e.g. lamp tab is only available to light sources).

For protein structures, I would recommend you reserve the imported structures as templates and only work with copies in the scene (just ctrl-c ctrl-v). It would be wise to first arrange the objects and only then worry about surface appearance and lighting.

Basic navigation: the numberpad will allow for basic viewing angles (x,y,z, camera). pressing and holding down middle mouse will allow for free panning around a rotation point. Double pressing '.' on the *numpad* will centre the rotation point on the selected object.

## Animating the scene


