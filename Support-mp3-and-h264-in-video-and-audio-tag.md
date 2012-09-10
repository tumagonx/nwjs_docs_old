For licensing issues node-webkit didn't ship with support for patented media formats, but for commercial cases where it's necessary to support patented media formats, we have solutions bellow.

**Please consult experts if you want to use patented media formats on your apps**

# Overview

Since node-webkit is based on Chromium, their media parts are basically the same. In order to open MP3 and H.264, you need to compile ffmpeg with corresponding features open. **And be careful, MP3 and H.264 codecs are GPL licensed in ffmpeg.**

But there is also a much simpler way, you can just copy those media codec files from Chrome.

**Notice: linking with GPL code will make your codes GPL too.**

# Windows

1. Locate Chrome\Application folder ( You can get to them pretty quickly by going to start > run > %localappdata% [enter] and then drilling down.)

2. Copy `avcodec-52.dll`, `avformat-52.dll`, and `avutil-50.dll` to your node-webkit distribution.

# Linux

The folder location may vary according to the linux distro you are using. Its located in the `/opt/google/chrome` on Ubuntu. Copy `libffmpegsumo.so` and paste it in the node-webkit folder.

# Mac

Head to your Applications folder and right-click Google Chrome. Choose show package contents and drill down to Versions > Most recent # > Framework > Libraries. Copy `libffmpegsumo.dylib`.

Then open `node-webkit.app/Content`