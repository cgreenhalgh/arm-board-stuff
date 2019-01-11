# Raspberry Pi Stuff

Initial testing with r-pi (model 1) B+ rev 1

## Raspbian image

Note: Raspbian isn’t and may never be 64 bit (which only affects r-Pi 3). 
Maybe see https://github.com/bamarni/pi64 (not yet 3B+?) and 
fork https://github.com/Crazyhead90/pi64 for 64 bit debian-based options

Downloaded Raspbian Stretch Lite from [here](https://www.raspberrypi.org/downloads/raspbian/)

351MB Zip. => 1.8GB image 

Copied onto a 16GB card; `lsblk`:
(this is on a VM with a USB card reader passed through to the VM)
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
...
sdb      8:16   1 14.7G  0 disk
├─sdb1   8:17   1 43.9M  0 part
└─sdb2   8:18   1  1.7G  0 part
```
i.e. sizes are consistent with initial image before first run.

`sudo blkid`:
```
/dev/sdb1: LABEL="boot" UUID="9304-D9FD" TYPE="vfat" PARTUUID="7ee80803-01"
/dev/sdb2: LABEL="rootfs" UUID="29075e46-f0d4-44e2-a9e7-55ac02d6e6cc" TYPE="ext4
" PARTUUID="7ee80803-02"
```

Note: for SSH support, [this](https://www.raspberrypi.org/documentation/remote-access/ssh/)
says "placing a file named ssh, without any extension, onto the boot partition of the SD card from another computer"
```
sudo mount /dev/sdb1 /mnt/sd0
sudo touch /mnt/sd0/ssh
sudo umount /mnt/sd0
sudo eject /dev/sdb
```

Note that the boot partition kernel.img/kernel7.img and says 
"Generated using pi-gen, https://github.com/RPi-Distro/pi-gen"

```
sudo mkdir -p /mnt/sd0
sudo mount /dev/sdb2 /mnt/sd0
```

Looks like a standard UNIX FS. Nothing in /boot though.

`df -k`:
```
/dev/sdb2        1712920  972736    635124  61% /mnt/sd0
```

`sudo fdisk -l`:
```
Disk /dev/sdb: 14.7 GiB, 15817768960 bytes, 30894080 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x7ee80803

Device     Boot Start     End Sectors  Size Id Type
/dev/sdb1        8192   98045   89854 43.9M  c W95 FAT32 (LBA)
/dev/sdb2       98304 3645439 3547136  1.7G 83 Linux
```

```
sudo umount /mnt/sd0
sudo eject /dev/sdb
```

### Boot on R-Pi

Default user 'pi'/'raspberry'

`uname -a`:
```
Linux raspberrypi 4.14.79+ #1159 Sun Nov 4 17:28:08 GMT 2018 armv6l GNU/Linux 
```
`fdisk -l`:
```
Disk /dev/mmcblk0: 14.7 GiB, 15817768960 bytes, 30894080 sectors   
Units: sectors of 1 * 512 = 512 bytes   
Sector size (logical/physical): 512 bytes / 512 bytes   
I/O size (minimum/optimal): 512 bytes / 512 bytes   
Disklabel type: dos   
Disk identifier: 0x9df17c54   
  
Device Boot Start End Sectors Size Id Type   
/dev/mmcblk0p1 8192 98045 89854 43.9M c W95 FAT32 (LBA)   
/dev/mmcblk0p2 98304 30894079 30795776 14.7G 83 Linux 
```
i.e. partition has been expanded to capacity of device.
`df -k` also shows file system has been expanded to partition.

## Docker

Follow install docker w convenience script 
[guide](https://docs.docker.com/install/linux/docker-ce/debian/#install-using-the-convenience-script)

`sudo usermod -aG docker your-user`

log out/back in

### Older R-Pis and Docker

Note that Docker does not officially support arm32v5 and v6 (including r-pi 1B+).
Arm32v7 and arm64v8 are support - 
see [docker](https://github.com/docker-library/official-images#architectures-other-than-amd64)

Note that there is an issue with older r-pi compatibility
At least for r-pi B+ rev 1, Need to downgrade docker, see [this issue](https://github.com/moby/moby/issues/38175)
```
sudo apt-get install docker-ce=18.06.1~ce~3-0~raspbian 
```

Hello-world image doesn't work with (e.g.) 1B+ - docker events shows exits with code 139.
Some other images do work though, e.g. 
- hypriot/armhf-hello-world
- hypriot/rpi-busybox-httpd
- arm32v6/busybox
