---
title: Package
char: random
short: Package your media file for use in any web server with mg.
---

The mg tool allows you to package statically your
 MP4 files. In this way you they can be effectively streamed on the Internet.
Packaging features:


[comment]: <> (https://en.wikipedia.org/wiki/HTTP_Live_Streaming">HTTP Live Streaming also known as HLS)
 * Statically package your mp4 files as:
 1. [Dynamic Adaptive Streaming over HTTP (DASH)](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP)
 2. [Http Live Stream (HLS)](https://en.wikipedia.org/wiki/HTTP_Live_Streaming)
 2. [Smooth Streaming](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming#Microsoft_Smooth_Streaming)



The mg tool allows to statically package a set of mp4 files. 
Mp4 files should have the key frames aligned.

Once you have [obtained](/download) mg you can run it using this parameters:

```bash
./mg -k:adaptive \
     -o:[output directory] \
     -i:[first input file] -s:0 -e:0 -b:750 \
     -j:[second input file] -b:350 \
     -j:[third input file] -b:120 \
     -j:[nth input file] -b:[nth bitrate]
```

[More MPEG-DASH and HLS packaging Information.->](https://github.com/mediagoom/mg/wiki/package)
