# Arch Linux Instalation in UEFI machine through system-d boot loader

## Partition your disk. Use your prefered way or see arch docs
Make sure to reserve a FAT32 partition with at least 500MB for the EFI system partition (ESP)

## Mount the created partitions

You will need at least:
- An ext4 partition for the base system
- A FAT32 partition for the UEFI system partition (ESP) Allocate at least 500MB.
- A swap partition.

Mount the base sytm partition to `/mnt` and the ESP to `/mnt/boot`

## Install base system

```bash
pacstrap /mnt base base-devel
```

## Set up boot loader

Install the intel updates package (if you use an intel processor) and then set up systemd-boot

```bash
pacman -S intel-ucode
bootctl --path=/boot install
```

Create the config file for the boot loader entry in `/boot/loader/entries/arch.conf`.
Before you open the file collect yout `/` partition's UUID, you can d that with the command
`ls -l /dev/disk/by-partuuid` get the UUID of the root partition and put it in the place of 
the `XXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXX` below.

```bash
title Arch Linux
linux /vmlinuz-linux
initrd /intel-ucode.img
initrd /initramfs-linux.img
options root=PARTUUID=XXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXX rootfstype=ext4 add_efi_memmap
```

* Fonts include:
https://fhackts.wordpress.com/2016/09/09/installing-archlinux-the-efisystemd-boot-way/
https://wiki.archlinux.org/index.php/microcode#Installation
http://www.rodsbooks.com/efi-bootloaders/principles.html
