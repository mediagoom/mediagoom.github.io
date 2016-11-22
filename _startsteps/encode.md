---
title: Encode
char: cog
short: Encode your files to several bitrates for Adaptive Streaming. 
---
First of all you need a media file you want to stream on the web.

In order to stream the file you need to encode the file to several mp4 files with differents bitrates and with key frames aligned.

If you do not know how to do it the simplest way is to use [ffmpeg](https://ffmpeg.org/download.html).

For each bitrate you want ran the following command:

```
ffmpeg -i "[your file path]" -vf "scale=w=1280:h=720" -codec:v libx264 -profile:v high -level 31 -b:v 750 -b:a 96k -codec:a aac -profile:a aac_low -r 25 -g 50 -sc_threshold 0 -minrate 750 -maxrate 750 -bufsize 750 -x264opts ratetol=0.1 mg_750.mp4
```

In the above command line make sure you replace [your file path] with the path of the file with the content you want to stream.

The above example produce a 750kb file. You can repeate the process with each bitrate you think is appropriate. 



