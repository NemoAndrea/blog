---
layout: post
title: Minimising the size of FreeCAD git repos 
---

## Background

Whether you are developing quick personal hardware designs, or working on a proper Open Hardware project, it is imperative that you use some kind of versioning system. This  will in the first place serve your own effort, as you can roll back to and keep track of important functional versions, but it also allows others to potentially collaborate or explore different ideas. 

While Git is not really designed for binary files such as FreeCAD's save files, it can certainly deal with them. Using delta compression that is built-in to Git, it can actually do a pretty good job of minimising the repository size when the changes between binary files are not too large. The limitations of Git when handling large binary files still apply, but most CAD projects are not likely to use gigabytes of data for the design files -- especially if only _source_ files are committed.

My prior approach to dealing with binary files and CAD in specific was to use Git-Annex. [Git-Annex](https://git-annex.branchable.com/) is very powerful and seems to be more aligned with the decentralised spirit of Git than the current favoured approach of Git-LFS when it comes to storing large files. Git-Annex, however, does involve overhead of managing a third-party storage, authentication and some potential additional failure points. So, it is worth seeing how bad things get when we just use plain ol' Git.

## FreeCAD format

[FreeCAD](https://www.freecad.org/) is a fanastic Open Source CAD suite that is getting better every day. FreeCAD save files (`.FCStd`) are nothing more than plain zip files containing a large range of objects (mostly `.brep`, some `.xml` and then the odd other files if you use other workbenches). Note that the order of files in the zip file matters -- so keep track of the zipped order when unzipping as otherwise you can't put the pieces back together into a functional FreeCAD file! You can set the level of compression for FreeCAD files in user preferences; from my understanding this is just the standard zip compression level; no special FreeCAD modification on top of this.

After going through some discussions on the FreeCAD forum, I wanted to run some simple verification tests to see if it would indeed make more sense to save FreeCAD files without compression, and let Git handle the compression instead. This is not a new idea, but I did want to run my own checks before deciding on the way forward. If FreeCAD is compressing the components of the zip file (i.e. the `.brep` and `.xml` files), then it would be very difficult or impossible for Git's delta compression to find any matching bits of binary data between commits. In the worst case scenario, you would have to add the full new binary file size in each commit to the size of the Git repo. In practice, not all `.brep` files would probably need to change between different commits (in a normal workflow where you change only a few chamfers and dimensions or add a new part), and therefore delta-compression would still be able to save some storage space on those compressed chunks that have not changed their content. At any rate, uncompressed would be expected to perform better.

Since delta compression might work even better if git can keep track of separate files (rather than everything lumped in a single zip/fcstd), I also wanted to see how the size would scale if you unzipped the FreeCAD file and commited those files to git. Note that this is not very practical; you would need to stitch these back together when cloning the repository. This can certainly be done with hooks, and scripts exist to achieve this, but I do not find this an adequate solution. But who knows, I might change my tune if it makes a huge difference in repo size...

To test this I created [a simple repo](https://codeberg.org/NemoAndrea/freecad-git-size-comparison) that you can clone your git repo containing FreeCAD designs into, and see the size difference between compressed an uncompressed format for the FreeCAD file. The script simply re-created your Git repo fresh, using the current version of FreeCAD to save the file, and copying the commits form the template repo. Have a look at the very simple python script to check the details.

## Results

*These results will likely be updated in the future as I run a few more repos through the script*

| Repo                                     | Unzipped (KiB) | Uncompressed (KiB) | Compressed (KiB) |
| ---------------------------------------- | ---------------- | -------------------- | ------------------ |
| anycubic-d2-rat-pack-filter-modification | 2760             | 3282 (+19%)          | 6874 (+149%)       |
| abzu-4-port-interposer                   | 3647             | 4659 (+28%)          | 9215 (+153%)       |


## Closing Thoughts

As expected, the results do seem to bear out that when it comes to minimising the size of the repository, using uncompressed FreeCAD files is the best choice, and that the difference is quite significant. I will definitely be changing my preferences. 

One minor downside is that the current checked out file is quite a bit larger. I don't think this will ever be a problem, but if this bothers you, you can write a [commit hook](https://github.com/hoijui/ReZipDoc) that commits the uncompressed version of your FreeCAD file, while you keep the compressed version in your working tree.

I don't know enough about Git's delta compression to know if the same benefits of unzipping the FreeCAD file before committing can be achieved on a single bundled file like an uncompressed zip file. I think that having a FreeCAD file represented by a directory will always be clumsy; and not worth the ~25% size decrease in my opinion. Unzipping would provide benefits when it comes to being able to selectively ignore files with `.gitignore`.

A future point of size reduction might be achievable through the selective omission of `.brep` files. These `.brep` files contain representations of geometry for Open Cascade. They also function a bit like pre-computed/cached geometry; having them means you don't need to recompute everything in the document when you open a FreeCAD file -- certainly a good feature to have. On the other hand, a lot of the geometry generated through the Part Design or Part workbench can be described in terms of basic operations (e.g. 'extrude face 32 by 50mm in ...'), and thus the final file size could, in principle, be much smaller. Unfortunately, some preliminary testing and [freecad forum discussions](https://forum.freecad.org/viewtopic.php?style=2&t=58880) seem to bear out that some `.brep` files are not merely chached geometry, and are in fact essential to a functional document. I think it will be an interesting direction for future exploration to see if we can deterministically figure out which `.brep` files can be recomputed from the basic operation commands[^1] of a FreeCAD file. I would certainly be happy to commit only the 'minimal' FreeCAD file to Git, and just suffer the extra recomputation time when checking out a prior commit.

> Some of these ideas are already [implemented in RealThunder's fork of FreeCAD](https://forum.freecad.org/viewtopic.php?p=498411#p498411). That fork is, in general, full of interesting ideas.

## Prior Art

_Some interesting writeups or links on this topic_

* Dante Catalfamo wrote [a blog post](https://blog.lambda.cx/posts/freecad-and-git/) in 2021 on using [Zippey](https://bitbucket.org/sippey/zippey/src/master/) to get useful git diff messages and more efficient storage. I haven't compared it, but since it requires the user of the repo to configure zippey, this approach does not quite meet my requirement of 'out of the box' functionality for new project contributors or users.
* The FreeCAD [forum discussion on version control](https://forum.freecad.org/viewtopic.php?t=38353)
* The [History addon Workbench by Ephi Blanshey](https://github.com/eblanshey/HistoryWorkbench) tries to implement some more content-aware git commits through a separate .yaml file and facilitate viewing diffs directly in FreeCAD. Useful as a proof of concept.


[^1]: which from my understanding are specified (in part) in `.xml` and `.txt` files in the FreeCAD zip file.