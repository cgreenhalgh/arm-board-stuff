# Raspberry Pi 64bit Stuff

## Boards

Databox is recommending R-Pi 3B+
- w. BCM2837B0 (1.4GHz) quad-core ARM Cortex A53 (ARMv8) cluster
- i.e. Armv8-A architecture
- Two execution states: AArch32 (to execute existing Armv7-A applications) and AArch64 (used by databox)
- Note, all general purpose Arm v8 devices have hardware FP support

## R-Pi Boot

See [docs](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/bootflow.md)

Defaults to SD card then USB (mass-storage devices then n/w DHCP/TFTP). 
Configurable via OTP (one-time programmable) to (also) use GPIO to select boot modes.
- checks each of the boot sources for a file called bootcode.bin
- finds first FAT partition (used to have to be first partition)
- ("also supports GUID partitioning"?)
- (some other stuff about booting from the USB OTG interface)

According to [this](https://raspberrypi.stackexchange.com/questions/10442/what-is-the-boot-sequence), 
after bootcode.bin we get:
- load and run start.bin, which load start.elf
- start.elf loads kernel.img
- It then also reads config.txt, cmdline.txt and bcm2835.dtb (optional)
- kernel.img is then run on the ARM.

## Alpine

Databox is using Alpine 64 bit (aarch64) as default host.

...
