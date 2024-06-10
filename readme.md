# Description

This repository has resources for launching Ubuntu 24.04 On a Qemu emulator with a 64 bit ARM architecture.

# Instructions

Run the setup script with `setup.sh`.
This will download and unpack the image as well as resize it
Be careful as this script can take a long time and uses 10 GB of disk space.

The setup script automatically runs the launch script but it can also be run with `launch.sh`.
This will launch the emulator with 4 GB of ram and port forwarding through port 22

Occasionally running the launch script fails. When this happens close the terminal and run the script in a new one. I've found that it usually works on the 2nd or 3rd try.

## Inside the emulator

It is recommended, though not required, to not update the initramfs.
This can be done by running `sudo nano /etc/initramfs-tools/update-initramfs.conf'
and then changing "update_initramfs=yes" to "update_initramfs=no".
Downloading some packages which requires updating the initramfs which can take a very long time.

# To do

Add a menu script for easier customization when setting up and launching.
Find why the launch script fails and find a solution.
