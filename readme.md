# Emulating SBCs on Ubuntu 24.04(Noble Numbat) with qemu.

This repository has instructions for emulating Ubuntu Noble on qemu with ARM64 architecture

## Installing qemu

Install qemu with ```sudo apt install qemu-system```

## Download The image

A list of Noble images can be found at ```https://cdimage.ubuntu.com/releases/24.04/release/```
Many of the images are compressed with xz. An example of how to download and unpack them:
```
wget https://cdimage.ubuntu.com/releases/24.04/release/<your-image>.img.xz
xz -d <your-image>.img.xz
```

## Resizing the image

Once you've downloaded and unpacked the image you *can* use it to launch an emulator but it will have very little disk space.You can increase the disk size by running 
```qemu-img resize <your-image>.img +10G```
This will increase the chosen image by 10 Gigabytes.
