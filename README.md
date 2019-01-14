# ARM Board Stuff

Noodling around with ARM boards, e.g. R-Pi, CubieTruck.
Maybe for Databox, maybe for music projects.

Chris Greenhalgh, Copyright (c) The University of Nottingham, 2019

See 
- [cubietruck](docs/cubietruck.md)
- [r-pi 32 bit](docs/rpi32.md)
- [r-pi 64 bit](docs/rpi64.md)

[Vagrantfile](Vagrantfile) is just a Debian stretch VM to try and make life easier on Windows.

Why?
- [bela](https://bela.io/) - note, 32bit, default software uses debian (stretch or wheezy)
- [databox](https://github.com/me-box/databox) - note, on arm recommends 64bit, [alpine](https://alpinelinux.org/downloads/) or [Hypriot](https://github.com/DieterReuter/image-builder-rpi64)

## VM/host

```
sudo apt update
```

```
lsblk
```
List block devices - includes SD Card.
