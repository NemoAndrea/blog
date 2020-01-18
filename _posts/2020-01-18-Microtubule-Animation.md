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

For the scences with a flaring microtubule, each protofilament (offset appropriately in z to get the 3 start helix) was animated by having each dimer follow a spline (see image below). Splines were offset in z to give the microtubule end a more ragged appearance, and the number of protofilaments was also varied in time and number between protofilaments. Timing was all done via keyframes (animate the z position of each spline, the number of repeats in array modifier etc). The resulting animation is not scientifically accurate of course (tip dynamics are stochastic), but it gives an impression. One could make it more accurate by using the built in python scripting in blender.

![animating a microtubule via splines](https://lh3.googleusercontent.com/zagaCUASsVc2vPUUAtrLIXBxStd_JLB5nA7pA173L9Mv4lt4j4KHErzJV68lQ_tEX7qVOptewSSPyeKZ84dN7bkqXOy0BJBERtmeoefV_CYKlyR7bdduQUIU0ZUr-JcfAGZ83CpIiwO3iLQtnINa70-hjALDrpKOo7hboEq1Gmb4MZKpQw5IkHo2MzAECMLpL8j7opv6AoNj7EkNln9YavdSK-RCzcfdqAQgBhTymoqkp_9_spHAZ0sVVOYCFawpe5IgZONS7yX5tL5OIfmZmeqc49ORsdcULUSjt5WX3ZbWqFhhnf6mO87duN7KDC-wcsnwbJbOKbnWXuMeMyQftuHpocOGZeO_OTCyD8KoTSJfaB3sIrglKngn5kpJE4CC6tL3xfenaN2M0NglVPBBI32xG1ybXyV43vLl7yXmDxshztoB_Ylpe1HlVIyj9A7gM0L9UszgQOfMZ2KlVXHjSe0BWKnaWo5wGyHWzrCGqLwtMGG6LLKhed0Zs_Sr2kgACA0pAyrbEVMcPQFyCyifNOZKygo6j9rkM0NkAmoGyZBtVlopRTGZ84Pj_yGq7KtDVF9chbK3jYOabhPubV98aKUkMHGudDk0JloNFAR6FT9dk8hYLqQbk1vZ4xWR-LAU3R9GBIFZZKk11PzDuEGwGxmAB8e-poVQsnr743SXTvx9l2JYPQ5vMwtk=w1498-h635-no)

## Microtubule shading

I have opted for a cartoon shader look for the microtubule. This was a very simple toon shader, which was laid out as in the image below. To get the black outine, a new black material was added, with a solidify modifier, and flipped normals. This flipped normals make it such that you only see material 'from the inside' of the object, which means you only see it on places where you would expect an outline.

![simple cartoon shader](https://lh3.googleusercontent.com/ldZ3NXeDWVEUJpoeQV9AGvQ1j8ICrLAGXhMwkqFblYYtY1pqrYLsKnwmlxVYwKsXJ8ES1HdVHq-YI9GIYsLa_RWYH59LzbpczLIXJaHpCY85NHNvzWrMXjHNUik8YJq1xmt2PdU3sf27UY3dg-gaDU90ABeZZR5cPAlhQzDgDHhQfiYfNRHy56Q2qf2QItKTfvrwG2Uf60BnMNgqdlt4h6JdOsoBrRFOK2XduRHetlnWa4eBVOXi0BK3fNKycnap886YBBL6PJOPdvDqy5K_XsUatNth53oob4sh2FOgtmIumK21u8uQJ45e2MMsR13cD1-PbdHafXzfDTev-yY6n16waLSwNKHfcgT5GK2Q_aXkZVHl3JQZF44C3TYrY91h4_6CiMv8El4PO2L7tGQXpGIKxWEtA5xgc67H0UmlVdqhR4640CHTnzIYpWyXD4aU7wRz7iQHvlO_30p-KDZ4XjZ4x2FIhmIjfFn97PaaogtWVvXfySLoOvcCrGOr77A1L6a7lpZ-GMUskih-hXdupkJpF2nhWUcrl9lnRKD8FC9jKLDPTcU9O5c7NikAwRd4UAvQomKw4ffPlnKGn7V2Ewbr3tOm3AdPcgkdfgbkGUDARCBtcua5PsbQKClyyiKAO2EkawHVPkUShRuscPqcFaK5XiHC4sVj2qQnV6JJz_Vrowx80Ia6s3V1=w1498-h833-no)

## Video export and Text

Blender outputs a series of png images, one for each frame. I used Davinci resolve (free software) to import these images and their
text functions and export settings to render the video. I use this software for timelapses, as it automatically
handles import of series of images into clips and handles export for youtube. 

![da vinci timeline](https://lh3.googleusercontent.com/NZqgY8YnIkhV07a_lFhImdrGdO-X-A1hskf0y9q3R2iLuV3Cq11j1vZ61VVnq_T_nuafGq6Q2d3RPmpaZJJTG9O54VvMIj9qo5wtYZlfw_wKqJlg2NOHNibXQGGyPLkWCYTZtXf6Iq1hxS-nNoPvqYlkefLVyhg11UnrjTAjeXF22jKpxsS4CUJQgzwa711ON2vVseMpoF7U2ent52KHXhQKAHcf4ZXZC4n-e48GfE1dPZhl_gYVFM_IDWjYjo8284P-8m2XoYoDAuybAR-dHAh3VSVlsTTkWtQwx3Qy7qPNzeQdTswyNFdbX0hZ62kaSqQLk3p9YiwuRkqTAKp-54H_i4HkuNfo2nqp12qBgsNIsqjsWcFLC9I_eAAN6SxPY72drJTh3tiVc1V5nHM4tFU_0udrhUzszJy_d1H2wIiS84y2c-YmTkGaozIVoCwyzb2LT-AOwrbIkmB00NRh5LJs27Ylx7PGLMRGUQ_KzGEgZKiLjjcKIOOaA2LFmtGxRd4shGMJO3hxgxXRQVHAsyeq-bRuBTmxedvcVVzJ3XGtzYJtEndb9Fv4KY0TtAc3UpZLDo15mUU6YC0a3xAMx8kFuNJz9cFt0t5MaccGxVPS7mnt8EgUZhrpGAu4oDWzih3ARQENn8Au0pjbaT8tCjMMvAApt2KV3GpctXoIBvhQJwI712DeN7pP=w1498-h505-no)

## What I would do differently next time

I would probably spend a bit more time reducing the vertex count before animating, as some scences were tricky to work with in terms of performance.
Blender has a problem that as soon as you keyframe (i.e. animate) any array modifier, it rebuilds the whole scene in every frame (not just the ones where the array modifer value changes) which made rendering and working with a scene like the microtubule growing and depolymerising take a long time. I would probably also work on making the growth and shrinking part more accurate by doing some scripting via python.
