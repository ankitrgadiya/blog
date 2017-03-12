---
layout: post
title: Arch Linux Installation
description: A Step-by-step guide to install Archlinux.
---

This is a Step-by-step tutorial for installing Arch Linux.

## Arch Linux
A Linux distro which is commonly considered for power users. It provides a lot of customisation right from the installation process. Its very light-weight distro, and it install very few core packages at the time of installation. Also one get all the latest packages as Arch Linux is rolling release distro. Also Arch users get access to Arch User Repository aka AUR which is a comprehensive repository with tons of packages. One can say that AUR contain all the packages that you can find on any other distro.

But the installation process is little complicated as you don't get any GUI installer instead you've to install it from command line only. So, here's the step by step guide to install Arch Linux on your machine.

## Steps:
<br />
* Download the Arch Linux iso from [here](https://www.archlinux.org/download/).

*Note: The iso is common for both 64 bit and 32 bit.*

* Burn the iso into a CD or make a bootable usb from the iso downloaded and boot from it.

* Choose your architecture from the boot menu.

* Once you get to the commandline, check your internet connection.
```bash
$ ping google.com
```
* List the disks
```bash
$ fdisk -l
```
* Choose the disk you want to install Arch Linux in and partition it.
```bash
$ cfdisk /dev/sdx
```
*Caution: This will erase all the data on that disk, choose wisely. I'm using "x" in sdx, you can replace it with variable corresponding to your disk.*

* Choose **dos**

* Create one root partition, one swap partition, and more if you need them.

* Make root partition bootable and change swap partition type to Linux Swap.

* Write and quit

* Change root partition to ext4 file system.
```bash
$ mkfs.ext4 /dev/sdx1
```
* Change swap partition file system to swap.
```bash
$ mkswap /dev/sdx2
```
* Mount swap partition.
```bash
$ swapon /dev/sdx2
```
* Mount all the partition.
```bash
$ mount /dev/sdx1 /mnt
```
*Note: Mount all other partitions if any.*

* Update pacman repositories.
```bash
$ pacman -Sy
```
* Install Arch base packages.
```bash
$ pacstrap -i /mnt base base-devel
```
* Generate fstab
```bash
$ genfstab -U -p /mnt >> /mnt/etc/fstab
$ cat /mnt/etc/fstab
```
* Change root into the new system
```bash
$ arch-chroot /mnt
```
* List all the regions and cities for time zone.
```bash
$ ls /usr/share/zoneinfo/
$ cd /usr/share/zoneinfo/REGION
$ ls
```
* Update your time zone using above information
```bash
$ ln -s /usr/share/zoneinfo/REGION/CITY /etc/localtime
$ hwclock --systohc --utc
```
* Generate locale
```bash
$ nano /etc/locale.gen
```
*Note: Uncomment your specific localization.*
```bash
$ locale-gen
$ echo LANG=en_US.UTF-8 > /etc/locale.conf
```
Replace *en_US.UTF-8* with the one you uncommented previously.

* Set hostname
```bash
echo HOSTNAME > /etc/hostname
```
Replace *HOSTNAME* with your hostname.

* Set the network configuration
```bash
$ systemctl enable dhcpcd@enpos3.service
```
* Set root password
```bash
$ passwd
```
* Create a new initial RAM disk
```bash
$ mkinitramfs -p linux
```
* Install bootloader
```bash
$ pacman -S grub os-prober
$ grub-install -target=i386-pc -recheck /dev/sdx1
$ grub-mkconfig -o /boot/grub.cfg
```
* Exit the chroot environment
```bash
$ exit
```
* Unmount the disks
```bash
$ umount -R /mnt
```
* Reboot
```bash
$ reboot
```
* Done!
