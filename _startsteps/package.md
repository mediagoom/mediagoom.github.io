---
title: Package
char: random
short: Package your media file for use in used in any web server using mg tool.
---

Package allows to repackage statically your
 MP4 files in order
to be used effectively streamed on the Internet.
Packaging features:
<ul>
    <li>Statically package your mp4 files as:
        <ul>
            <li><a href="https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP">Dynamic Adaptive Streaming over HTTP (DASH)</a></li>
            <!--
            <li><a href="https://en.wikipedia.org/wiki/HTTP_Live_Streaming">HTTP Live Streaming (also known as HLS)</a></li>
            -->
            <li><a href="https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming#Microsoft_Smooth_Streaming">Smooth Streaming</a></li>
        </ul>
    </li>
</ul>


The mg tool allow to statically package a set of mp4 files. 
Mp4 files should have the key frames aligned.

Once you have [optained](/download) mg you can run it using this parameters:

```bash
./mg -k:dash \
     -o:[output directory]
     -i:[first input file] -s:0 -e:0 -b:750
     -j:[second input file] -b:350
     -j:[therd input file] -b:120
     -j:[nth input file] -b:[nth bitrate]
```

[Read More.->](https://github.com/mediagoom/mg/wiki/package)
