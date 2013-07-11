For licensing issues node-webkit didn't ship with codec for patented media formats, but for commercial cases where it's necessary to support patented media formats, we have solutions bellow.

**Please consult experts if you want to use patented media formats on your apps.**

# Overview

Since node-webkit is based on Chromium, their media parts are basically the same. In order to open MP3 and H.264, you need to compile ffmpeg with corresponding features open. **And be careful, MP3 and H.264 codecs are GPL licensed in ffmpeg.**

There is also a simpler way, you can try to use the media codec files from Chrome. It had been reported to work, but we don't test this and it may not work for you in latest version.

**Notice: linking with GPL code will make your codes GPL too.**

# Windows

You need files from the matching Chrome version. (0.6.x is based on Chromium 28)

1. Locate Chrome\Application folder (e.g. C:\Program Files\Google)

2. Copy `ffmpegsumo.dll` to your node-webkit distribution.

# Linux

The folder location may vary according to the linux distro you are using. Its located in the `/opt/google/chrome` on Ubuntu. Copy `libffmpegsumo.so` and paste it in the node-webkit folder.

# Mac

Head to your Applications folder and right-click Google Chrome. Choose show package contents and drill down to Versions > Most recent # > Framework > Libraries. Copy `libffmpegsumo.dylib`.

Then open `node-webkit.app/Contents/Frameworks/node-webkit Framework.framework/Libraries/` and paste the `libffmpegsumo.dylib` file.

# build your own
Alternatively, if you are building node-webkit, open src/third_party/ffmpeg/chromium/scripts/build_ffmpeg.sh, go to (approximately, might change) line 379 and change
```sh
# Google Chrome & ChromeOS specific configuration.
add_flag_chrome --enable-decoder=aac,h264,mp3
add_flag_chrome --enable-demuxer=mp3,mov
add_flag_chrome --enable-parser=aac,h264,mpegaudio
```
To

```sh
add_flag_common --enable-decoder=aac,h264,mp3
add_flag_common --enable-demuxer=mp3,mov
add_flag_common --enable-parser=aac,h264,mpegaudio
```

Then follow the short directions here:
http://src.chromium.org/svn/trunk/deps/third_party/ffmpeg/README.chromium

Please also at least turn on the resource loader's support in Chromium's code, or your format will be treated as non supported MIME type and won't be loaded. See src/net/base/mime_util.cc. You might want to look into code in other files guarded by 'USE_PROPRIETARY_CODECS' macro.