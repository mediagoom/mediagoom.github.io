---
layout: default
title: test
---

<pre>

ffmpeg -i <b>"[your file path]"</b> -vf "scale=w=1280:h=720" -codec:v libx264 -profile:v high -level 31 -b:v 750 -b:a 96k -codec:a aac -profile:a aac_low -r 25 -g 50 -sc_threshold 0 -minrate 750 -maxrate 750 -bufsize 750 -x264opts ratetol=0.1 mg_750.mp4
</pre>




----------

[PLAY]: https://cdn.rawgit.com/mediagoom/Play/v0.0.3/index.html?src=https://cdn.rawgit.com/mediagoom/Play/v0.0.2/bb "Media Goom Sample Player"

[TAR]: https://dl.dropboxusercontent.com/u/33964970/bbb.tar "sample files"

Online test [player][PLAY]
Online [sample files][TAR] 


```powershell
use-python
use-gyp
gyp **mgtest.gyp** \-\-depth 0 \-Duv_library=static_library \-Dtarget_arch=ia32 \-I../deps/libuv/common.gypi

**../mg/configure \-\-enable\-debug**
```

----------

Running just one test:

```bash
env TESTS="UV" make \-e check
```

----------
```bash

url=https://dl.dropboxusercontent.com/u/33964970/bbb.tar

if [ \-e bbb.tar ]; then echo "tar exist"; else curl \-O $url ; fi

tar xvf bbb.tar 


\#\# declare an array variable
declare -a arr=(                      \\
\'bbb_1024x576_h264-750Kb_aac-lc.mp4\'  \\
\'bbb_1280x720_h264-1200Kb_aac-lc.mp4\' \\
\'bbb_1280x720_h264-2000Kb_aac-lc.mp4\' \\
\'bbb_1280x720_h264-3500Kb_aac-lc.mp4\' \\
\'bbb_512x288_h264-320Kb_aac-lc.mp4\'   \\
)

declare -a br=('750' '1200' '2000' '3500' '320')

l=${#arr[@]}

ou="$(pwd)/kk"

if [ -d $ou ]; then rm "$ou/*"; else mkdir $ou; fi

cmd="-k:ssf -o:$ou"

for (( c=0; c<$l; c++ ))
do  
if [ "$c" == "0" ]; then cmd+=" -s:0 -e:0 -i:"; else cmd+=" -j:"; fi

cmd+="${arr[$c]}";
cmd+=" -b:${br[$c]}";
done

#echo $cmd

echo "now run ./src/mgcli/mg \$cmd"




#if($enc)
#then
#	$cmd += " -key:3a2a1b68dd2bd9b2eeb25e84c4776668 -kid:10000000100010001000100000000001 `"-playreadyurl:http://playready.directtaps.net/pr/svc/rightsmanager.asmx?PlayRight=1&amp;UseSimpleNonPersistentLicense=1`" -senc_flags:0  -clearkey:true"
#fi

echo $cmd






```

```bash

url=https://dl.dropboxusercontent.com/u/33964970/mg.tar.gz

if [ -e mg.tar.gz ]; then echo "tar exist"; else curl -O $url ; fi

tar -xvzf mg.tar.gz

```


```powershell



$ff     = 'C:\tmp\ffmpeg.exe'
$inp    = 'f:/iomega/newmedia/bbb_sunflower_2160p_60fps_normal.mp4'
$outdir = 'c:\tmp'
$name   = 'bbb'

if(-not (test-path $ou))
{
md $ou
}else
{
ls $ou | rm
}

function encode($n, $ff, $f, $o, $k, $w=1280, $h=720)
{

& $ff -i "$f" -vf "scale=w=$($w):h=$($h)" -codec:v libx264 -profile:v high -level 31 -b:v "$($k)k" -b:a 96k -codec:a aac -profile:a aac_low -r 25 -g 50 -sc_threshold 0 -minrate "$($k)k" -maxrate "$($k)k" -bufsize "$($k * 1)k" -x264opts ratetol=0.1 "$outdir\$($n)_$($w)x$($h)_h264-$($k)Kb_aac-lc.mp4"



}


encode $name $ff $inp $outdir 750 1024 576
encode $name $ff $inp $outdir 3500 1280 720

encode $name $ff $inp $outdir 320 512 288

encode $name $ff $inp $outdir 120 256 144

encode $name $ff $inp $outdir 1200 1280 720
encode $name $ff $inp $outdir 2000 1280 720



```

```powershell


function Expand-ZIPFile($file, $destination)
{
$shell = new-object -com shell.application
$zip = $shell.NameSpace($file)
foreach($item in $zip.items())
{
$shell.Namespace($destination).copyhere($item)
}
}


$mp4 = './mg'
$enc = $false;
$ou  = '.\BBB'
$dir = $pwd
$url = 'https://dl.dropboxusercontent.com/u/33964970/bbb.zip'


if(-not (test-path "$dir\bbb.zip"))
{
Invoke-WebRequest -Uri $url -OutFile "$dir\bbb.zip"
Expand-ZIPFile "$dir\bbb.zip" "$dir"
}

if(-not (test-path $ou))
{
md $ou
}else
{
ls $ou | rm
}


$ff = @(
'bbb_1024x576_h264-750Kb_aac-lc.mp4'
, 'bbb_1280x720_h264-1200Kb_aac-lc.mp4' 
, 'bbb_1280x720_h264-2000Kb_aac-lc.mp4' 
, 'bbb_1280x720_h264-3500Kb_aac-lc.mp4'
, 'bbb_512x288_h264-320Kb_aac-lc.mp4'
, 'bbb_256x144_h264-120Kb_aac-lc.mp4'
);

$bb = @(750, 1200, 2000, 3500, 320, 120); 

$cmd = "-k:ssf -o:$ou "

for($i = 0; $i -lt $ff.length; $i++)
{
if(0 -eq $i)
{
$cmd += " -s:0 -e:0 -i:"
}
else
{
$cmd += " -j:"
}

$cmd += "`"$dir\$($ff[$i])`"";
$cmd += " -b:$($bb[$i])"
}

if($enc)
{
$cmd += " -key:3a2a1b68dd2bd9b2eeb25e84c4776668 -kid:10000000100010001000100000000001 `"-playreadyurl:http://playready.directtaps.net/pr/svc/rightsmanager.asmx?PlayRight=1&amp;UseSimpleNonPersistentLicense=1`" -senc_flags:0  -clearkey:true"
}

$cmd | oh

(Measure-Command {Invoke-Expression "& '$mp4' $cmd"}).ToString()

#ls $ou

foreach($b in $bb){"$($b): " | oh; (ls $ou | ? {$_.Name -match "video_$($b)000_"} | measure-object -maximum -property 'length').maximum / 2 * 8 / 1024}

```



