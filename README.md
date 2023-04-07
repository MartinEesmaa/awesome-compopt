# Awesome archive compressors & optimizers
A list of the awesome archive compressor and optimizer programs.

## Contributing
You're welcome to add additional compressor/optimizer tools and anything by a pull request.

## Table of contents
- [Audio](#audio)
- [Video](#video)
- [Web](#web)
- [Image](#image)
- [Archive](#archive)
- [Mux](#mux)
- [Executable](#executable)

## Audio
*The audio compressors is for use lossless music and speech audio files.*

### Lossless compressors
* [FLAC](https://xiph.org/flac/) - Free Lossless Audio Codec, best for compatability
* [ALAC](https://en.wikipedia.org/wiki/Apple_Lossless_Audio_Codec) - Apple Lossless Audio Codec, best for Apple legacy devices.
* [TAK](http://thbeck.de/Tak/Tak.html) - Tom's Lossless audio Kompressor, best for speed of encode & decode
* [WavPack](https://wavpack.com) - Wave Packer, best for DSD and 32-bit float IEEE
* [OptimFROG](http://losslessaudio.org/) - Optim Frog, best for file size of maximum preset
* [LA](https://web.archive.org/web/20210501091140/http://www.lossless-audio.com/) - Lossless Audio Codec, best alternative to OptimFROG
* [Monkey's Audio](https://monkeysaudio.com/) - Monkey's Audio, best for compression well.
* [MP4-ALS](https://en.wikipedia.org/wiki/Audio_Lossless_Coding) - MPEG-4 Audio Lossless, best for multichannel compression ratio.
* [TTA](http://tausoft.org/wiki/True_Audio_Codec_Overview) - True Audio Lossless, best for ultra low latency.
* [SAC](https://github.com/slmdev/sac) - State of the Art Lossless Codec, best alternative to OptimFROG.
### Physical/hardware media lossless compressors
* [Dolby TrueHD](https://en.wikipedia.org/wiki/Dolby_TrueHD) - Used in Ultra HD Blu-ray releases, but some publishers uses that audio codec.
* [DTS-HD Master Audio](https://en.wikipedia.org/wiki/DTS-HD_Master_Audio) - Most popular for the Bluray releases.
* [Meridian Lossless Packing](https://en.wikipedia.org/wiki/Meridian_Lossless_Packing) - Alternative same as Dolby TrueHD, but it is useful for DVD Audio and HD-DVD.
* [LPCM](https://en.wikipedia.org/wiki/Pulse-code_modulation#Implementations) - Mandatory used in DVD and Blu-ray.
* [WMAL](https://en.wikipedia.org/w/index.php?title=Windows_Media_Audio#Windows_Media_Audio_Lossless) - Windows Media Audio Lossless, best for legacy Windows & Microsoft hardwares.

### Repack compressor of lossy audio files
* [packMP3](http://packjpg.encode.su/?page_id=19) - A compression program for further compressing MP3 audio files (most compatability and FOSS cross-platform)
* [mpz](https://mega.nz/file/f08U0aAC#jPgr1vdMAi8cXYEyiQTXIQeE7bD5jgxuU8tnEYrOzm8) - Sound Slimmer of MP3 repack compressor, but the development ended in 2007 (trialware)
* [OGGRE](https://darckrepacks.com/files/file/262-oggre/) - OGGRE is a vorbis lossless compressor (freeware)

### Optimizers
* [MP3packer](https://wiki.hydrogenaud.io/index.php/MP3packer) - MP3packer is a program which can rearrange the data within an MP3 to fulfill specific goals for reducing 2-10% from CBR to VBR without a loss quality and can rearrange back anytime for CBR or VBR MP3 audio files, but likely optimizer.
* [OptiVorbis](https://github.com/OptiVorbis/OptiVorbis) - Library and application for lossless, format-preserving, two-pass optimization and repair of Vorbis data, reducing its size without altering any audio information.

### Availability & compatability

Compressors | FOSS | Platform | FFmpeg (cross-platform)
--- | --- | --- | --- |
FLAC | ✔️ | Cross-platform | ✔️ |
ALAC | ✔️ | Cross-platform | ✔️ |
TAK | ❌ | Windows | ✔️ (decode only) |
WavPack | ✔️ | Cross-platform | ✔️ |
OptimFROG | ❌ | Windows, Mac, Linux, FreeBSD | ❌ |
LA | ❌ | Windows & Linux | ❌ |
Monkey's Audio | ❌ | Windows | ⚠️ (maximum 2 channels decode only)
WMAL | ❌ | Windows | ⚠️ (partially decode support)
MPEG-4 ALS | ✔️ | Cross-platform | ✔️ (decode only) |
TTA | ✔️ | Cross-platform | ✔️ |
SAC | ✔️ | Cross-platform | ❌ |

Note about SAC: It does not support error detection, you should compress `sac --encode example.wav example.sac` and decompress SAC audio to WAV to match MD5 decompressed same as original WAV MD5 by `sac --decode example.sac example.wav`. If both MD5 were same, it's lossless, but if both MD5 were different, it's loss quality when using SAC Lossless codec.

**Hardened audio internal program commands:**
```
flac -8ep -r 15 -l 32 --lax file.wav
takc -e -p4m file.wav
wavpack -x6hh file.wav
ofr --preset max --seek slow file.wav
la -high -noseek file.wav
mac file.wav file.ape -c5000
mp4als -o1023 -a -b -7 -p -MP4 -v file.wav
tta -e file.wav file.tta
sac --encode --sparse-pcm --optimize=high file.wav file.sac
```

**FFmpeg hardened commands:**
```
ffmpeg -i file.wav -c:a flac -compression_level 12 -lpc_type cholesky -lpc_passes 8 -exact_rice_parameters 1 file.flac
ffmpeg -i file.wav -c:a wavpack -compression_level 13
ffmpeg -i file.wav -c:a tta file.tta
```

**MP3Packer:**

MP3Packer compresses and rearranges for CBR/VBR MP3 Layer I/II streams, but likely optimizing.

```
mp3packer --copy-time -z constant.mp3
```

**packMP3:**

packMP3 can compress MP3 files with Layer I only

```
packmp3 example.mp3
```

**MPZ:**

mpz can compress MP3 files with any layers and error frames without loss, but it's closed source for Windows available and trialware. The binary file requires MpzSlimmer.dll

Encode (e)/decode (d):
```
mpz e example.mp3 example.mpz
mpz d example.mpz example.mp3
```

Add the lines in Arc.ini
```
[External compressor:mpz]
header    = 0
packcmd   = mpz.exe e $$arcdatafile$$.mp3 $$arcpackedfile$$.mpz
unpackcmd = mpz.exe d $$arcpackedfile$$.mpz $$arcdatafile$$.mp3
datafile = $$arcdatafile$$.mp3
packedfile = $$arcpackedfile$$.mpz
```

Recommended options compress via FreeArc:
```
arc a -m0 MP3MPZ.arc -mmpz example.mp3
```

**OGGRE:**

OGGRE can compress Vorbis streams only, but it's closed source for Windows available and freeware.

It is recommended to use FreeArc for compressing and decompressing.

Please test it to decompress and match MD5 decompressed same as original MD5.

For decompressing, please use cls-oggre.dll in FreeArc.

Add the lines in Arc.ini
```
[External compressor:oggre]
header = 0
packcmd = oggre_enc.exe {options} $$arcdatafile$$.tmp $$arcpackedfile$$.tmp
```
Recommended options compress via FreeArc:
```
arc a -m0 VORBIS.arc -moggre example.ogg
```

## Video
*The video lossless compressors is with video archiver.*

* [AVC](https://en.wikipedia.org/wiki/X264)
* [HEVC](https://en.wikipedia.org/wiki/X265)
* [VVC](https://en.wikipedia.org/wiki/Versatile_Video_Coding)
* [VP9](https://en.wikipedia.org/wiki/VP9)
* [AV1](https://en.wikipedia.org/wiki/AV1)

**AVC:**

Encode lossless video of x264 or FFmpeg:
```
x264 --qp 0 --preset placebo original.y4m -o original.h264
```
```
ffmpeg -i original.y4m -c:v libx264 -preset placebo -crf 0 lossless.mp4
```
**HEVC:**

Encode lossless video:
```
x265 --lossless --preset placebo original.y4m -o original.h265
```
```
ffmpeg -i original.y4m -c:v libx265 -preset placebo -x265-params lossless=1 -crf 0 lossless.mp4
```

**VVC:**

Encode lossless video via Fraunhofer HHI vvenc encoder:

```
vvencffapp --CostMode lossless -i original.y4m --preset slower -b result.266
```

For x266, Multicoreware is releasing x266 encoder soon in H2 2023, but future versions will support lossless. Learn more: https://multicorewareinc.com/faq-x266-webinar/

**VP9:**

Encode lossless video:

```
vpxenc --best --cpu-used=-9 --lossless=1 original.y4m -o result.ivf
```
```
ffmpeg -i original.y4m -c:v libvpx-vp9 -deadline best -cpu-used -8 -lossless 1 -speed -16 -quality best result.ivf
```

**AV1:**

***aomenc is the only one lossless encoder***

Encode lossless video:
```
aomenc -i original.y4m --lossless=1 --cpu-used=3 -o result.ivf
```
```
ffmpeg -i original.y4m -c:v libaom-av1 -aom-params lossless=1 -cpu-used 3 -o result.ivf
```

## Web

* [Tidy](https://github.com/htacg/tidy-html5)
* [JSMin](https://github.com/douglascrockford/JSMin)
* [wasm-opt](https://github.com/WebAssembly/binaryen#wasm-opt)

**Tidy:**

Tidy can optimize HTML, CSS and XML.

```
tidy --clean true sample.html > samplemin.html
```

**JSMin:**

JSMin can optimize javascript files:
```
jsmin <example.js >example.min.js
```

**wasm-opt:**

wasm-opt can optimize WebAssembly files:
```
wasm-opt -Oz example.wasm
```

## Image

* [packJPG](https://github.com/packjpg/packJPG) - A compression program for further compressing JPEG image files
* [Lepton](https://github.com/dropbox/lepton) - Lepton is a tool and file format for losslessly compressing JPEGs by an average of 22%.
* [JPEG-XL](https://github.com/libjxl/libjxl) - JPEG XL image format reference implementation
* [ECT](https://github.com/fhanau/Efficient-Compression-Tool) - Fast and effective C++ file optimizer
* [paq8px](https://github.com/hxim/paq8px) - PAQ8PX compression archiver
* [Scour](https://github.com/scour-project/scour) - An SVG Optimizer / Cleaner
* [gifsicle](https://github.com/kohler/gifsicle) - Create, manipulate, and optimize GIF images and animations

**packJPG:**

packJPG can compress JPG files fast, most compatability and cross-platform.

```
packjpg example.jpg
```

**Lepton:**

Lepton is the Dropbox JPG compressor tool, but the development has deprecated since November 19 2022. Two binaries are lepton default and lepton slow best ratio. On Lepton default, the JPG file is compressed little bigger than packJPG compressed file, but the slow best ratio got little better than packJPG.

```
lepton -skipvalidate example.jpg
lepton-slow-best-ratio -skipvalidate example.jpg
```

**JPEG-XL:**

JPEG-XL can compress JPG and PNG files, even supports animations and lossless/lossy compression. JPEG-XL was implementated to the [supported softwares](https://jpegxl.info/), but due to the cons for Chrome version 110+ removed JPEG-XL and Firefox is only available version of Nightly. JPEG-XL lossless compression ratio of JPG files got a little bigger file size and memory than packJPG and Lepton, but my best suggestion is for PNG files and the supported programs.

Default command:
```
cjxl -d 0 example.png example.jxl
```

Hardened command:
```
cjxl -d 0 -e 9 -E 11 -g 3 -I 100 example.png example.jxl
```

If you would like to get best file size possible of effort 10, you could add command `--allow_expert_options -e 10`, this could take very slower process and may increase memory usage.

**ect:**

Efficient Compression Tool can optimize JPG and PNG files, command:
```
ect -9 example.jpg
ect -9 example.png
```

Hardened commands:
```
ect -80085 --allfilters-b --pal_sort=120 example.png
```

Note: When optimizing bigger picture resolutions, it may increase CPU and memory usage and all filters & pal sort is only for PNG files.

You could enable `--mt-deflate --mt-file` to speed up process for increasing more CPU and memory usage.

Documentation: https://github.com/fhanau/Efficient-Compression-Tool/blob/master/doc/Manual.docx

**paq8px:**

paq8px can compress more better compression ratio using JPEG/PNG/BMP built in model than packJPG, Lepton and JPEG-XL, but it is more slower process than other JPG compressors.

```
paq8px -7ba example.jpg
```

NOTE:

Level 8 and above may not work for 32-bit operating systems and gives out of memory.

Some JPEG files may not detect, which goes back to normal model.

**Scour:**

Scour is the cleaner and optimizer of SVG files.

Normal:
```
scour -i input.svg -o output.svg
```
Better (for older versions of Internet Explorer):
```
scour -i input.svg -o output.svg --enable-viewboxing
```
Maximum scrubbing (hardened):
```
scour -i input.svg -o output.svg --enable-viewboxing --enable-id-stripping \
  --enable-comment-stripping --shorten-ids --indent=none
```

TIP: You could output SVGZ compressed file `-o output.svgz` and optimize using ECT: `ect -80085 -gzip output.svgz` to save file size.

**gifsicle:**

gifsicle can optimize your GIF files.
```
gifsicle example.gif
```

## Archive

* [7-Zip](https://www.7-zip.org/)
* [WinRAR](https://www.win-rar.com/)
* [FreeArc](https://web.archive.org/web/20160124195439/http://freearc.org/)
* [PeaZip](https://peazip.github.io/)
* [paq8px](https://github.com/hxim/paq8px)

## Mux

* [MKVToolNix](https://mkvtoolnix.download)
* [FFmpeg](https://ffmpeg.org)
* [mp4box](https://gpac.wp.imt.fr/)

To optimize a little file sizes:

**MKVToolNix:**

Mux one file to matroska container using GUI and command line to remove all metadata, including tags:

```
mkvpropedit file.mkv --tags all: --edit info --set writing-application="" --edit info --set muxing-application="" -d date -d segment-uid
```

**FFmpeg:**

You can remove writing library and metadata:

```
ffmpeg -i example.mp4 -c copy -fflags +bitexact -flags:v +bitexact -flags:a +bitexact -map_metadata -1 example_removed.mp4
```
For AVC, add `-bsf:v "filter_units=remove_types=6"`

For HEVC, add `-x265-params no-info=1`

**mp4box:**

You can remove writing library and metadata:

```
mp4box -itags all=NULL -for-test -add example.h264 -new example.mp4
```

## Executable

* [UPX](https://github.com/upx/upx) - the Ultimate Packer for eXecutables
* [Crinkler](https://github.com/runestubbe/Crinkler) - Crinkler is an executable file compressor (or rather, a compressing linker) for compressing small 32-bit Windows demoscene executables.
* [Leanify](https://github.com/JayXon/Leanify)
* [MSVC](https://visualstudio.microsoft.com/vs/features/cplusplus/) - Microsoft Visual C++
* [GCC](https://gcc.gnu.org/) - the GNU Compiler Collection
* [Clang](https://clang.llvm.org/) - a C language family frontend for LLVM

To compress executable file:

**UPX:**

You can compress using the best method for supported version programs:

```
upx --best --ultra-brute --all-methods example.exe -oexample_shrinked.exe
```

If you would like to compress all icons, add `--compress-icons=3`

See the UPX documentation: https://github.com/upx/upx/blob/devel/doc/upx-doc.txt

**Crinkler:**

Note: This can only target .obj files, maximum 8 KB size, C/C++/ASM codes and Windows 32-bit via Visual Studio 2003+ version or Intel C++ Compiler before the final compiles to output a shrinked executable file.

For Visual Studio .NET 2003 version: Use /GA to reduce file size.

For Visual Studio 2005 and later: Use /GS- to reduce file size.

Crinkler normal command:
```
cl /c /O1 /GS- example.c && \
crinkler /ENTRY:main /SUBSYSTEM:CONSOLE example.obj kernel32.lib
```

Crinkler insane command:
```
cl /c /O1 /GS- example.c && \
crinkler /ENTRY:main /NODEFAULTLIB /UNSAFEIMPORT /TINYHEADER /TINYIMPORT /COMPMODE:VERYSLOW /HASHSIZE:500 /HASHTRIES:100 /SUBSYSTEM:CONSOLE example.obj kernel32.lib user32.lib
```

See the Crinkler documentation: https://github.com/runestubbe/Crinkler/blob/master/doc/manual.txt

If your code wants to target subsystem Windows, change from CONSOLE to WINDOWS.

**Leanify:**

***Please do UPX compress first, before you head to Leanify and test the program***

Leanify can remove only 512 bytes for Windows 32-bit and 64-bit UPX portable executable packed file for removing header after compress. This cannot unpack UPX packed file forever after Leanify shrinked UPX packed file, but it still runs ok.

```
leanify upx_packed.exe
```

**Visual Studio:**

If you want to compile example C file, command:

```
cl /O1 example.c /link user32.lib
```

**GCC:**

If you want to compile example C file, command:
```
gcc -O3 -flto example.c
```

**Clang:**

If you want to compile example C file, command:
```
clang -O3 example.c
```

**TIP:** If you're making a small math calculator in console and it's lower than 2048 MB memory reported in Task Manager/System Monitor, this must be 32-bit build to save up file size than 64-bit build.

If you're making huge like game graphics, this must be 64-bit build.

**TIP 2:** Use minimum compiler to compile acceptable codes.

**TIP 3:** Remove dead codes using Cppcheck for C++ codes or Visual Studio.

- Martin Eesmaa