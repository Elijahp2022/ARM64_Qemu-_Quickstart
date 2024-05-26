# Running a 64 bit ARM machine on qemu

Most likely when you installed qemu it installed ARM64 support but in case it didn't you can run ```sudo apt install qemu-system-arm```
I am using a Raspberry Pi server image from ```https://cdimage.ubuntu.com/releases/24.04/release/ubuntu-24.04-preinstalled-server-arm64+raspi.img.xz``` but if you decide to use a different image make sure to change the commands accordingly.
## Extracting the kernel and initrd image

If you do not already have fdisk installed run ```sudo apt install fdisk```
Then we can see the partitions of the image by running ```fdisk -l ubuntu-24.04-preinstalled-server-arm64+raspi.img```
The partitions can differ depending the image and some other factors but for me the initrd and kernel were in the first partition.

To find the offset by multiplying the sector size(most likely 512) by the start of the first partition.
Then mount the partition by running ```sudo mount -0 offset=<your-1st-offset> ubuntu-24.04-preinstalled-server-arm64+raspi.img /mnt/<your-mount-point>```

From here we can copy the kernel and initrd by running
```
cp /mnt/<your-mount-point>/vmlinuz .
cp /mnt/<your-mount-point>/initrd.img .
```

While we're here we can also mess with the network-config file to allow for wifi connection on our first boot. Or we can do this from inside the VM after we've launched it.

## Setting the root filesystem

Unmount the first partition then mount the second partition like we did before
```
sudo umount /mnt/<your-mount-point>
sudo mount -o offset=<your-2nd-offset> ubuntu-24.04-preinstalled-server-arm64+raspi.img /mnt/<your-mount-point>
```
Then go into ```/mnt/<your-mount-point>/etc/fstab``` with your favorite text editor.
Edit the root filesystem to be /dev/vda2 and alll other file systems to be routed through /dev/vda1
This is another thing that can differ based on image and other factors but it should end up with a line that looks something like ```/dev/vda2		/ ext4 and so on``` and a line that looks like ```/dev/vda1	/boot/firmware vfat and so on```

## Launching the VM

The VM is booted with one very long command which is in the launch.sh file. You can boot the VM by running ```sudo bash launch.sh```

## Internet Setup

We can setup a network connection by going into editing ```/etc/netpan/xx-cloud-init.yaml```
First run ```ip addr``` to see the name of your wifi interface(It will most likely be wlan0 or enp0sxx). Then insert the following below the ```network:``` line:
```
renderer: networkd
wifis:
    <wifi-interface-name>:
      access-points:
        "<SSID>":
          password: "<PASSWORD>"
```

After that run '''sudo netplan apply''' and reboot the VM by running '''sudo poweroff''' and running the launch script again.
