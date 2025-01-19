# wine-window-fix
  
This is a patched PKGBUILD for fixing the reparent issues X11 child windows face under the new wine mouse driver

**DISCLAIMER**: I did not develop the patch, I just adapted the code to the newer version. This might cause other issues on X11 based applications, feel free to open an Issue if this is the case, I might give it a shot fixing it  

**THIS IS EQUIVALENT TO INSTALLING A PACKAGE FROM THE AUR, YOU WILL NEED TO MANUALLY UPDATE IT**
  
### Motivation

This is a band-aid fix for wine that attempts to fix a regression that presents using yabridge plugins in various DAWs (and possibly CARLA).  
This repo aims to be as close as possible as the upstream Arch packages and should replace wine and be replaced by it whenever needed without any fiddling necessary.

### Known bugs  
- Window menus might appear relative to screen absolute (0, 0) coordinates instead of where they are supposed to be

### Usage

#### Direct install

The compiled package is provided for convenience, and should be reproducible.

Grab the latest pkg.zst from the Releases section, then install it as root with 

```bash
pacman -U *filename*
```
  
It will ask to replace your current wine install, then the UIs should mostly be fixed

##### Notes on package signing

My inital intent was to provide a signed package as release but it involved fiddling with pacman-key and adding my pgp key to it, and I assumed some people might not be comfortable with that 
  
#### Manually compile the PKGBUILD

No specific fiddling should be necessary. Grab my signature with the same procedure as the *Direct Install* method, then compile the whole thing with  
```bash
makepkg -i 
```

#### AUR
Aur package coming soon

### Sources
- https://bugs.winehq.org/show_bug.cgi?id=51357 - Where the patch has ben picked from
- https://gitlab.archlinux.org/archlinux/packaging/packages/wine - The base Arch package

