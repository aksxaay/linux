okay i'm trying to use Nvidia on demand because using it all on the gpu is kinda wack.

run with gpu command works but doesn't work completely
`brave://gpu` says almost everything only runs on software

[this stackoverflow // choose gpu for application](https://askubuntu.com/questions/596855/choose-which-gpu-to-use-for-each-application)

[how nvidia on-demand works // stackoverflow](https://askubuntu.com/questions/1201072/how-nvidia-on-demand-option-works-in-nvidia-x-server-settings)

this doesn't cover everything but this certain reply does manage to cover all bases. \


**default GPU**
```
glxinfo | grep "OpenGL renderer"
```

**set GPU**
```
DRI_PRIME=1 glxinfo | grep "OpenGL renderer"
```
output
```
libGL error: failed to create dri screen
libGL error: failed to load driver: nouveau
OpenGL renderer string: Mesa Intel(R) UHD Graphics 620 (KBL GT2)
```
this is a problem.

for nvidia-smi to work i have to apt install `nvidia-utils-515`

[nvidia-prime-offload](https://download.nvidia.com/XFree86/Linux-x86_64/440.44/README/primerenderoffload.html)
this is some documentation.


```
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxinfo | grep vendor
```

opening `brave-browser` this way simply does not work.

idk how to monitor the other thing.

theres
`intel-gpu-tools` apt software

I also found out that I can shift my desktop icons left and right as long as i'm on desktop goddamn.

someone here was unable to offload chrome on PRIME
[prime offloading unable on chrome](https://forums.developer.nvidia.com/t/prime-offloading-unable-to-run-chrome/81600/6)


```sh
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia /usr/bin/google-chrome-unstable --ignore-gpu-blacklist --enable-gpu-rasterization --enable-native-gpu-memory-buffers --enable-zero-copy
```
okay here's a weird one off command.

[ask fedora gpu offloading firefox / chrome](https://ask.fedoraproject.org/t/how-to-use-gpu-offloading-with-browser-firefox-or-chromium/6317)

firefox needs EGL?
failed to create EGL surface
this one particular firefox has EGL support?
[egl firefox](https://mastransky.wordpress.com/2021/10/30/firefox-94-comes-with-egl-on-x11/)

this is all that's left now

[firefox video accell // reddit](https://www.reddit.com/r/firefox/comments/olz0um/unable_to_make_firefox_use_video_acceleration_on/)

[wiki arch // firefox hardware video acceleration](https://wiki.archlinux.org/title/Firefox#Hardware_video_acceleration)

discord doesn't work on this offload process as well.


only blender works with this version like thing.
man I remember reaching this same conclusion and things bruh this sucks.

this should try and tell us the current state of nvidia drivers on Linux.


There was this [comfy guide Nvidia Optimus](https://www.youtube.com/watch?v=Pn2iUgW3l6w) on linux
and this is pretty much the best video I have seen.

[denshi wiki nvidia](https://wiki.denshi.org/hypha/software/nvidia)
