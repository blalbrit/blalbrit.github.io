---
layout: post
title: "Re-uniting Palimpsest Pages in IIIF"
date: 2017-07-07
tags : [IIIF, manuscripts, palimpsests, presentationAPI, Mirador]
author:
  login: blalbrit
  email: blalbrit@stanford.edu
  display_name: Benjamin Albritton
  first_name: Benjamin
  last_name: Albritton
---
{% include JB/setup %}

#Re-uniting Palimpsest Pages in IIIF

## Overview

In our work with the [Biblioteca Apostolica Vaticana](http://digi.vatlib.it/), we have been experimenting with a particular use-case in the "digital manuscript reconstruction" space. Specifically, when a manuscript was scraped clean and then repurposed to receive new text, the original leaves were often folded and inserted into new quire structures. For a scholar working with the undertext of a palimpsest, it is extremely useful to be able to manipulate the disjunct leaves in order to virtually reconstruct the original writing surface. [IIIF](http://iiif.io) theoretically makes this possible, though in practice it is clear that additional tooling will be necessary to completely satisfy scholarly use-cases.

We're very fortunate to be able to collaborate with Dr. András Németh and his colleagues at the Vatican, as their use-cases will help inform development work within the IIIF sphere that will be generalizable to other areas of study. Dr. Németh has provided four examples, drawn from the Vatican collection, of digital pages that need to be manipulated in various ways in order to support study of the undertext of several palimpsests.

In the four examples presented here, the issues of reconstruction and representation can be modeled in IIIF and, to greater or lesser success, viewed through existing software. I'll start with two fairly simple examples, where two full images need to be fitted back together and rotated.

### Example 1: Vat.gr.103 - ff. 142v and 147r

In this instance, we need to create a single [canvas](http://iiif.io/api/presentation/2.1/#canvas) that includes the two surfaces (ff. 142v and 147r), and then rotate them by 90 degrees clockwise in order to see the two column undertext in its original configuration.

We can address each image separately thanks to IIIF as follows:

![alt text](http://digi.vatlib.it/pub/digit/MSS_Vat.gr.103/iiif/Vat.gr.103_0298_fa_0142v.%5B02.wl.0000%5D.jp2/full/pct:10/0/native.jpg "142v: http://digi.vatlib.it/pub/digit/MSS_Vat.gr.103/iiif/Vat.gr.103_0298_fa_0142v.%5B02.wl.0000%5D.jp2/full/pct:10/0/native.jpg")
142v: "http://digi.vatlib.it/pub/digit/MSS_Vat.gr.103/iiif/Vat.gr.103_0298_fa_0142v.%5B02.wl.0000%5D.jp2/full/pct:10/0/native.jpg" 

and 

![alt text](http://digi.vatlib.it/pub/digit/MSS_Vat.gr.103/iiif/Vat.gr.103_0308_fa_0147r.%5B02.wl.0000%5D.jp2/full/pct:10/0/native.jpg "147r: http://digi.vatlib.it/pub/digit/MSS_Vat.gr.103/iiif/Vat.gr.103_0308_fa_0147r.%5B02.wl.0000%5D.jp2/full/pct:10/0/native.jpg")
147r: "http://digi.vatlib.it/pub/digit/MSS_Vat.gr.103/iiif/Vat.gr.103_0308_fa_0147r.%5B02.wl.0000%5D.jp2/full/pct:10/0/native.jpg" 

For demonstration purposes, I have limited the images to 10 percent of their overall size ("pct:10" in the URLs). However, we will be dealing with the full size image or, to be honest, the largest size that the Vatican will deliver. To determine the largest image size delivered from a IIIF server, we can use the image API pattern that delivers information about the image: "http://digi.vatlib.it/pub/digit/MSS_Vat.gr.103/iiif/Vat.gr.103_0298_fa_0142v.%5B02.wl.0000%5D.jp2/info.json". 

The results of this request, give us: 

```
{
"tiles": [
{
"width": 512,
"height": 512,
"scaleFactors": [
1,
2,
4,
8
]
}
],
"protocol": "http://iiif.io/api/image",
"sizes": [
{
"width": 307,
"height": 438
},
{
"width": 614,
"height": 877
},
{
"width": 1228,
"height": 1755
}
],
"profile": [
"http://iiif.io/api/image/2/level1.json",
{
"formats": [
"jpg"
],
"qualities": [
"native",
"color",
"gray"
],
"supports": [
"regionByPct",
"sizeByForcedWh",
"sizeByWh",
"sizeAboveFull",
"rotationBy90s",
"mirroring",
"gray"
]
}
],
"width": 2456,
"@context": "http://iiif.io/api/image/2/context.json",
"height": 3510
}```

From this, we see that the overall size delivered for this image is 2456w x 3510h. A quick check of the other image we are working with shows dimensions of 2456w x 3508h. 

We know, then, that our composite canvas, which includes both images, will be 2456 x 2 = 4912w and 3510h (the larger of the two image heights). Per Dr. Németh's instructions, this is what the composite canvas should look like:

![alt text](http://localhost:4000/assets/palimpsest_example1.png)

Our task, then is to create a IIIF canvas that can accommodate both images. We'll create a stand-alone [manifest](http://iiif.io/api/presentation/2.1/#manifest) for this example. See [here](http://dms-data.stanford.edu/data/manifests/test/test1/manifest.json) to examine the full manifest in detail. Ignoring the preamble, let's look directly at the new canvas:

```{
     "@id": "https://dms-data.stanford.edu/data/manifests/BAV/palimp-test1/canvas/canvas-01",
     "@type": "sc:Canvas",
     "label": "Example 1",
     "width": 4912,
     "height": 3510,
     "images": [
       {
         "@id": "http://digi.vatlib.it/iiif/MSS_Vat.gr.103/res/anno0298",
         "@type": "oa:Annotation",
         "motivation": "sc:painting",
         "resource": {
           "@id": "http://digi.vatlib.it/iiif/MSS_Vat.gr.103/res/img0298",
           "@type": "dctypes:Image",
           "format": "image/jpeg",
           "service": {
             "@context": "http://iiif.io/api/image/2/context.json",
             "@id": "http://digi.vatlib.it/iiifimage/MSS_Vat.gr.103/Vat.gr.103_0298_fa_0142v.%5B02.wl.0000%5D.jp2",
             "profile": "http://iiif.io/api/image/2/level2.json"
           },
           "width": 2456,
           "height": 3510
         },
         "on": "https://dms-data.stanford.edu/data/manifests/BAV/palimp-test1/canvas/canvas-01#xywh=0,0,2456,3510"
       },
       {
         "@id": "http://digi.vatlib.it/iiif/MSS_Vat.gr.103/res/anno0308",
         "@type": "oa:Annotation",
         "motivation": "sc:painting",
         "resource": {
         "@id": "http://digi.vatlib.it/iiif/MSS_Vat.gr.103/res/img0308",
         "@type": "dctypes:Image",
         "format": "image/jpeg",
         "service": {
         "@context": "http://iiif.io/api/image/2/context.json",
         "@id": "http://digi.vatlib.it/iiifimage/MSS_Vat.gr.103/Vat.gr.103_0308_fa_0147r.%5B02.wl.0000%5D.jp2",
         "profile": "http://iiif.io/api/image/2/level2.json"
       },
       "width": 2456,
       "height": 3508
     },
     "on": "https://dms-data.stanford.edu/data/manifests/BAV/palimp-test1/canvas/canvas-01#xywh=2456,0,2456,3508"
   }
  ]
}```

### Example 2: Vat.gr.19 - ff. IVv and 1r


### Example 3: Vat.gr.984 - ff. 162r and 157v


### Example 4: Vat.gr.984 - ff. 169av and 169r


