# wine-window-fix
  
This is a patched PKGBUILD for fixing the reparent issues X11 child windows face under the new wine mouse driver

**DISCLAIMER**: I did not develop the patch, I just adapted the code to the newer version. This might cause other issues on X11 based applications, feel free to open an Issue if this is the case, I might give it a shot in fixing it  

**THIS IS EQUIVALENT TO INSTALLING A PACKAGE FROM THE AUR, YOU WILL NEED TO MANUALLY UPDATE IT**
  
### Motivation

This is a band-aid fix for wine that attempts to fix a regression that presents using yabridge plugins in various DAWs (and possibly CARLA).  
This repo aims to be as close as possible as the upstream Arch packages and should replace wine and be replaced by it whenever needed without any fiddling necessary.

### Known bugs  
- Window menus might appear at screen absolute (0, 0) coordinates

### Usage

#### Direct install

The compiled package is provided for convenience and is signed by my key 

Grab the latest pkg.zst.sig from the Releases section, fetch my PGP signature with  
```bash 
gpg --keyserver keyserver.ubuntu.com --recv-keys 3AFEAEEB 
```
  
then install it as root with 
```bash
pacman -U *filename*
```
  
It should automatically install and replace your current wine
  
#### Manually compile the PKGBUILD

No specific fiddling should be necessary. Grab my signature with the same procedure as the *Direct Install* method, then compile the whole thing with  
```bash
makepkg -i 
```

#### AUR
Aur package coming soon

