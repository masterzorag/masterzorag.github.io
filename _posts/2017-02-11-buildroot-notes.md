---
layout: post
title:  "Buildroot notes"
date:   2017-02-11 12:12:12 -0600
categories: install notes
---
## buildroot notes

- do not run as root  
```
blackbox ~ # login user  
$ mkdir br_ppc64

$ wget https://buildroot.uclibc.org/downloads/snapshots/buildroot-snapshot.tar.bz2  
$ tar xf buildroot-snapshot.tar.bz2  
$ cd buildroot  
```

* We build out-of-tree, just setup once!  
`$ make O=/home/user/br_ppc64`

* Move back, now we have the Makefile to build out-of-tree!  
`$ cd /home/user/br_ppc64`

### we TARGET a (real or emulated) pp64
* Import a cfg  
`$ make defconfig BR2_DEFCONFIG=/home/user/br_ppc64/defconfig`

* Make changes  
`make menuconfig`

### build
`$ make`
* We got kernel and initramfs as compressed cpio!


### Optional
* Strip kernel  
```
$ export PATH=${PATH}:$(pwd)/host/usr/bin
$ powerpc64-linux-strip -s images/vmlinux
```
* Or  
`$ host/usr/bin/powerpc64-linux-strip -s images/vmlinux`


### Booting on real TARGET
* Use current bootload (yaboot, GRUB2)
* From OpenFirmware (Forth command prompt, TFTP)


### Emulating
* Test on emulated qemu-system-TARGET
```
qemu-system-ppc64 \
  -kernel images/vmlinux
  -initrd images/rootfs.cpio.xz \
  -vnc :2 -vga std -m 512 \
  -netdev user,id=net0 -device virtio-net-pci,netdev=net0 \
  -device virtio-keyboard-pci
```

### Now bootstrap Fedora ppc64
* Todo
