What doesn't work on your side?

# Audio
Audio should work. The problem should be the codec for mp3 or other non-free media formats. See [[Support mp3 and H264 in video and audio tag]]

# WebGL
2 extra DLL files from DirectX are needed, or your GFX card/driver is on Chromium's black list. See https://github.com/rogerwang/node-webkit/issues/185

Sometimes the css doesn't display correctly, such as `-webkit-transform:translateZ(-1000px);`. Maybe it's the problem with the WebGL.

# Lack of libudev.so.0
Maybe node-webkit doesn't work on your computer with the error.

````
nw: error while loading shared libraries: libudev.so.0: cannot open shared object file: No such file or directory
````

See https://github.com/rogerwang/node-webkit/issues/136. and the temporary solution [[The solution of lacking libudev.so.0]]