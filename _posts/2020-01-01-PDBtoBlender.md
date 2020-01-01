---
layout: post
title: Working with PDB files in Blender
---

In this article, I will go over about how I go about loading pdb structures into blender. 

Blender is freely available software that will run on Windows, Mac OS, and Linux. It is intendend for complex 3D animation 
and rendering. Because of this, Blender has every possible option you could imagine for creating renders that fit your needs. 
The downside is that it will have a steep learning curve. 

Since I like molecular biology, I figured I should try to import protein structures which are deposited at the 
[RSCB Protein DataBase](https://www.rcsb.org/) (PDB for short). With the recent new version of blender (2.8) 
rendering can now be done in realtime, which is a huge boost to user friendlyness and ease of operation. Luckily, I had this [blog post by Damien Farrell](https://dmnfarrell.github.io/bioinformatics/proteins-blender) to serve as a starting point.

## Importing into blender

To get started, we first need to go from a nice structure in PDB to something we can play around with in blender. 
As Damien Farrell mentions, [PyMOL](https://pymol.org/2/) will work fine for doing this. For some reason it keeps
talking about trials and a pro version, but I've not had the free version stop working so that should be fine. 
Alternatively, you can use [USCF Chimera](https://www.cgl.ucsf.edu/chimera/) for this. Both pieces of free software will allow
you to import a PDB structure by simply providing the 4 character code listed in the PDB entry 
(e.g. [rcsb.org/structure/6TF9](http://www.rcsb.org/structure/6TF9)). Some structures have additional 'bioassemblies' within the entry (i.e. particular arrangements of the molecule); if you want to load these, you'll have to manually download the PDB file or similar filetypes.

So, once the structure is open in either chimera or PyMOL, youll start working on exporting them to a format blender can import. By default, both programs display the protein in a 'ribbon' format. This is instructive, but to get a surface rendering you'll have to manually switch to this in both programs. In chimera, take care to turn off the ribbon rendering, otherwise you will also export this (ribbons will be hidden by the protein surface, so you won't notice).

Some specific things to look out for:
* In chimera, large structures will often fail or only partially work when you try to load a surface rendering. Splitting the individual chains (type 'split' in command line) will help a bit in this regard. For large structures I go to PyMOL, which seems to not have this problem.
* In my experience, PyMOL seems to produce models that have more vertices than chimera, even though they look equally detailed in blender. I'm sure this could be fixed, but If I can help it I therefore import from chimera

Once you have the surface rendering open in either program you will want to export them.
For Chimera got to [file] -> [export scene] -> save as .x3D file. For PyMOL go to  [file] -> [export as] -> save as vrml 2.
