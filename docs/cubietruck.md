# Cubietruck stuff

## General

Cubietruck has Armv7 Hard FP (32 bit, of course)

(I've got 2x CubieTruck NAND, i.e. CubieBoard 3 v1.0-0606 NAND)
e.g. [docs](http://dl.cubieboard.org/model/CubieBoard3/CubieBoard3%20Nand%20Version/Hardware/soc/A20%20user%20manual%20V1.0%2020130322.pdf)
with:
- Based on AllWinner A20 inc. Dual Cortex-A7 – ARMv7, Thumb2, VFPv4, “h/w virtualisation support”

## Boot

Note cubietruck boot presumes SD card has:
- MBR, within from 8kb, allows up to 4 partitions
- Initial loader (boot0), starts at 8kb, up to 32kb
- U-boot, starts at 40kb.

Typically works with single ext4 partition following (hidden) u-boot on SD card.
Custom bootloader and u-boot provided by allwinner/sunxi?!

## Armbian

See [Armbian debian](https://docs.armbian.com/User-Guide_Getting-Started/#what-to-download)
which supports quite a few ARM boards including Cubietruck.

Using CLI debian (stretch) mainline version (kernel 4.19.y), 
i.e. for headless server, light desktop operations (no video acceleration)

Recommends writing SD with [etcher](https://www.balena.io/etcher/).
Works reliably on Windows and MacOS in my experience.

Note: also consider using official SD formatter for card that has been used
to reset low-level usage flags on SD card.

Boots from SD by default with SSH enabled, root/1234. Forced change on login. 
Also expands image to full SD on first use.

wireless (integrated) set-up
```
sudo nmtui
```

Armbian uses a single partition (ext4) with kernel image and initial ramdisk in
/boot. `fdisk -l`:
```
Disk /dev/mmcblk0: 29.5 GiB, 31657558016 bytes, 61831168 sectors                
Units: sectors of 1 * 512 = 512 bytes                                           
Sector size (logical/physical): 512 bytes / 512 bytes                           
I/O size (minimum/optimal): 512 bytes / 512 bytes                               
Disklabel type: dos                                                             
Disk identifier: 0x6c9eb767                                                     
                                                                                
Device          Boot Start    End Sectors Size Id Type                       
/dev/mmcblk0p1       8192 61212831 61204640 29.2G 83 Linux      
```
`uname -a`:
```
Linux cubietruck 4.19.13-sunxi #5.69 SMP Wed Jan 9 15:42:24 CET 2019 armv7l GNU/Linux
```

## Docker

install docker w [convenience script](https://docs.docker.com/install/linux/docker-ce/debian/#install-using-the-convenience-script)

Seems to work fine, e.g. docker run hello-world (arm32v7 is officially supported)
