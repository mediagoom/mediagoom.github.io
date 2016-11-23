---
title: Encode
char: cog
short: Encode your files to several bitrates for Adaptive Streaming. 
---
First of all you need a media file you want to stream on the web.

In order to stream the file you need to encode the file to several mp4 files with different bitrates and with key frames aligned.

If you do not know how to do it the simplest way is to use [ffmpeg](https://ffmpeg.org/download.html).

For each bitrate you want ran the following command:

<pre>

fmpeg -i <b>"[your file path]"</b> -vf "scale=w=<b>1280</b>:h=<b>720</b>" \
-codec:v libx264 -profile:v high -level 31 -b:v <b>750</b> \
-b:a 96k -codec:a aac -profile:a aac_low -r 25 -g 50 -sc_threshold 0 -x264opts ratetol=0.1 \
-minrate <b>750</b> -maxrate <b>750</b> -bufsize <b>750</b> mg_750.mp4

</pre>

In the above command line make sure you replace [your file path] with the path of the file with the content you want to stream.

The above example produce a 750kb file. You can repeat the process with each bitrate you think is appropriate. 

You should modify any bold parameter with the most appropriate for you.



