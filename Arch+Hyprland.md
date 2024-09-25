# Steps to install Arch in PC
make flash boot and boot from it

## connecting to internet
connect to internet
  ```sh
  iwctl
  ```
connect to your wifi or ethernet
  ```sh
  device list
  station <inteface name(wlan0)> scan
  station <interface name(wlan0)> get-netwrork
  station <interfacee name(wlan0)> connect <network name(IIST-STUDENT-WIFI)>
  # then enter password
  exit
  ```
check network
  ```sh
  ip address
  ```
  this shoul be UP and a valid IP should be assiggned to your PC

## Creating Partitions
Run follwing command to get a list of all storage devices and their partitions
```sh
lsblk
```
Use following command to change partitions of any disk
I am creating 4 partition with following details
|---|---|---|
|name: nvme0n1p1| size: 500 MiB| type: EFI System| 
|name: nvme0n1p2| size: 376 GiB| type: Linux root|
|name: nvme0n1p3| size: 95  GiB| type: Linux filesystem|
|name: nvme0n1p4| size: 5   GiB| type: Linux swap |
|---|---|---|ß
```sh
cfdisk </dev/disk_name(/dev/nvmeon1)>
# use arrows keys to make partitions as your choice
# [Write] to save the partition table
# [Quit] to get back to the linux prompt
```
again check using ```lsblk```

Mount EFI and root partitions
```sh
mount /dev/<root_partition_name(nvme0n1p2)> /mnt
mkdir -p /mnt/boot/efi
mount /dev/<efi_partition name(nvme0n1p1)> /mnt/boot/efi
lsblk
``` 

## Installling Linux and its firmwares
```sh
pacstrap /mnt base linux linux-firmware
```
then generate and write fstab in /mnt/etc/fstab
```sh
genfstab -U /mnt >> /mnt/etc/fstab
```
and check /mnt/etc/fstab
```sh
cat /mnt/etc/fstab
```

## Setting up user in arch-chroot 
```sh
arch-chroot /mnt
useradd -m -G wheel -s /bin/bash <username(abhishek)>
passwd <username(abhishek)>
```
## settting up keyring and installing sudo package from pacman
```sh
pacman-key --init
pacman-key --populate archlinux
pacman -S sudo
pacman -S nano
pacman -S networkmanager
``` 

## givng sudoer permission
```sh
EDITOR=nano visudo
```
uncomment ```# %wheel ALL=(ALL) ALL``` to give sudo permission to all users in wheel group
save and exit uing Ctrl+S and then Ctrl+X 
change user using
```sh
su - <username(abhishek)>
```
check permission using ```sudo -i```

## installing bootloader
```sh
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

make sure you hav installed networkmanager and given sudo access to your user and setted up password also... without yhese 3 thing you wont be ale to access our newly installed setup

## Exit Chroot and Reboot
** Very important to save files properly
```sh
exit #to exit from chroot environment
umount -R /mnt #unmount all partition
reboot
```
