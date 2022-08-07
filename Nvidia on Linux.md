Hopefully this is where I'll explain how fucking difficult it is to use Hybrid Graphics Card on a Laptop.


### Keywords
- gpu offloading


[reddit thread// prime render offloading](https://www.reddit.com/r/linuxmasterrace/comments/garoie/what_is_nvidia_prime_render_offloading_and_which/)

[wiki // arch linux using optimus manager](https://wiki.archlinux.org/title/NVIDIA_Optimus#Using_optimus-manager)

out of nowhere I just found this gem

0

[choose which GPU to use for each application](https://askubuntu.com/posts/1306140/timeline)

If you want to select a specific GPU for the **entire session**, then selecting it with Prime, logging out and then logging back in will suffice.

However, if you really want to choose which GPU to use **for each application**, then you must use the `DRI_PRIME` environment variable for such specific application(s) instead of using the system-wide Prime setting.

Ideally, it's recommended to use the Prime program in order to preset the integrated GPU (graphics processing unit) as the default (system-wide) one, i.e. it's recommended to set the integrated Intel graphics card as the default one, because it saves power, helps keeping your computer operating at a lower temperature et cetera. Then when you decide to run that very demanding 3D appplication, you use `DRI_PRIME` to tell the system that it must use your Nvidia GPU to run such specific application.

As explained e.g. [here](https://askubuntu.com/questions/791022/how-to-configure-an-application-to-always-run-with-dri-prime-1-set-is-there-an) and [here](https://askubuntu.com/questions/757586/my-amd-radeon-graphic-card-is-not-working-on-16-04), once you've used Prime to "tell" the system that your integrated Intel GPU is the default one to be used system-wide, you tell your Nvidia GPU to run the specific application by running the command:

```
DRI_PRIME=1 specific-application
```

...where `specific-application` is e.g. a 3D modelling application or a 3D game or any other program that requires a GPU with more powerful graphics processing capabilities. If e.g. you want your Nvidia GPU to process the graphics of the [Inkscape](https://inkscape.org/) application, just run:

```
DRI_PRIME=1 inkscape
```

In order to run [Blender](https://www.blender.org/) using your Nvidia GPU instead of the integrated Intel GPU, just run:

```
DRI_PRIME=1 blender
```

Suppose that you want Blender to always use your Nvidia GPU instead of the integrated Intel GPU. In such case, you may use a text editor (such as GEdit, Nano, Mousepad, Leafpad etc.) in order to edit Blender's `.desktop` file:

```
sudo gedit /usr/share/applications/blender.desktop
```

...and then replace this:

```
Exec=/usr/bin/blender %u
```

...with this:

```
Exec=DRI_PRIME=1 /usr/bin/blender %u
```

...then just save the file, exit the text editor and start Blender by clicking on its icon at the Applications menu. Each one of those application icons at the Applications menu are one `.desktop` file, so if you modify e.g. `blender.desktop` you're pretty much modifying the Blender application shortcut at your Applications menu so when you click on such shortcut it runs Blender with `DRI_PRIME` set as `1` (i.e. use the Nvidia GPU to run Blender).

In case you don't know which is the current default GPU, run this command:

```
glxinfo | grep "OpenGL renderer"
```

And in order to see which GPU is activated when you set `DRI_PRIME=1`, run this command:

```
DRI_PRIME=1 glxinfo | grep "OpenGL renderer"
```

If your integrated Intel GPU is already set as the default (system-wide) GPU, then the first command (`glxinfo | grep "OpenGL renderer"`) will inform you that Intel is the GPU ran by `glxinfo` by default, and the second command (`DRI_PRIME=1 glxinfo | grep "OpenGL renderer"`) will inform you that Nvidia is the GPU ran by `DRI_PRIME=1 glxinfo` (i.e. Nvidia is not the default / system-wide GPU, but `DRI_PRIME=1` successfully set Nvidia as the _active_ GPU for that specific process initiated by `DRI_PRIME=1 glxinfo | grep "OpenGL renderer"`).




### Steps
- `DRI_PRIME=1 glxinfo | grep "OpenGL renderer"`
> libGL error: failed to create dri screen
libGL error: failed to load driver: nouveau
OpenGL renderer string: Mesa Intel(R) UHD Graphics 620 (KBL GT2)

yeah that is sad.


nvm I got it, just run `watch nvidia-smi` and it tells everything that is using the GPU
was able to run blender just fine like this.