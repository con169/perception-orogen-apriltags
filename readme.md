# Apriltag orogen Master 22.06 FIX!
This is a patched version of the `apriltag` library for compatibility with ROCK. It addresses integration issues with library linking and including sources from deprecated versions of Apriltag.

# How to replace the default /perception/orogen/apriltags with this new fix
```
cd ~/rock/perception/orogen/apriltags
rm -rf ./*
rm -rf ./.* 
git clone https://github.com/con169/perception-orogen-apriltags.git .
cd ~/rock
amake
```
## Tested With
- Ubuntu 20.04
- ROCK master 22.06
- OpenCV 4.2
- Orocos Toolchain

### Based on
This repo originates from [perception-orogen-apriltags](https://github.com/rock-perception/perception-orogen-apriltags) from the rock perception repo