---
layout: post
title:  "Working on HLS implementation"
date:   2017-01-18 13:29:16 +0100
categories: mg
#excerpt: HLS. Http Live Streaming. How mg support it.
comments: true
---


As some of you may know at the moment the most adopted Adaptive Streaming implementation are MPEG-DASH and HLS.
The mg tool already support packaging a series of *key frame aligned mp4* files to *MPEG-DASH*. 
However, this functionality alone does not allows you to reach all major browsers and all major devices.
In order to do so you need to package your *key frame aligned mp4* to *HLS* as well.
Since we aim to provide a complete solution for web streaming we are working on implementing HLS besides MPEG-DASH.

<!--excerpt-->

So we are hard at work to reach our goal. At this point we already have something working. 
In case you are interested you can use our dev-win branch in windows or our dev-nx branch in linux to build a version of mg capable of packaging to HLS.

If you do not want the complicacy of building the package yourself, you can use these link to get a developer build.

Linux build of mg for packaging to HLS:

```bash
curl https://dl.dropboxusercontent.com/u/33964970/mg -o
chmod +x mg
```

Windows link:
[Download windows build with HLS support of mg](https://ci.appveyor.com/api/buildjobs/hp52n8o4ststuvak/artifacts/win-release.zip)

Once you have buildt or download the mg version supporting HLS you can use it exactly as you would do with MPEG-DASh just use *-k:hls* instead of *-k:dash* in the command line.

For a [complete description of how using mg head to our wiki](https://github.com/mediagoom/mg/wiki).

Please give us feedback in case you use this feature.


