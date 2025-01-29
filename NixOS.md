# Installing NixOS

## Creating boot media
- Download .iso of minimal NixOS from official website
- Create NixOS bootable

## Installing
- Boot with our freshly created usb flash
- Select install minimal from the list
- Elevate to sudo when prompt come
```sh
sudo -i
```

## Creating Partitions
Run follwing command to get a list of all storage devices and their partitions
```sh
lsblk
```
Use following command to change partitions of any disk
I am creating 4 partition with following details

| name | size | type |
| --- | --- | --- |
| nvme0n1p1 | 500 MiB | EFI System | 
| nvme0n1p2 | 376 GiB | Linux root |
| nvme0n1p3 | 95  GiB | Linux filesystem |
| nvme0n1p4 | 5   GiB | Linux swap |

```sh
cfdisk </dev/disk_name(/dev/nvmeon1)>
# use arrows keys to make partitions as your choice
# [Write] to save the partition table
# [Quit] to get back to the linux prompt
```
again check using ```lsblk```

format the root partition and other partitions
```sh
mkfs.ext4 /dev/<root_partition(nvme0n1p2)>
mkfs.ext4 /dev/<other_partition(nvme0n1p3)>
```
format the EFI partition
```sh
mkfs.fat -F32 /dev/<EFI partition(nvme0n1p1)>
```
make swap partiton and activate it
```sh
mkswap /dev/<swap_partition(nvme0n1p4)>
swapon /dev/<swap_partition(nvme0n1p4)>
```

[!partition_image (partitions.jpg)]

Mount EFI and root partitions
```sh
mount /dev/<root_partition_name(nvme0n1p2)> /mnt
mkdir -p /mnt/boot
mount /dev/<efi_partition name(nvme0n1p1)> /mnt/boot
mkdir /mnt/imp
mount /dev/<file_system_partition_for_important_documents>(nvme0n1p3)> /mnt/imp
lsblk
```

## Generate the initial NixOS configuration
```sh
nixos-generate-config --root /mnt
```
- Edit the main configuration file
```sh
nano /mnt/etc/nixos/configuration.nix
```
- Make the following essential changes in the configuration file
  - Set the bootloader device:
    boot.loader.grub.device = "/dev/sda"; # or "nodev" for EFI only
  - Configure networking (e.g., for Wi-Fi):
    networking.networkmanager.enable = true;
  - Set up a user account:
    users.users.yourusername = {
      isNormalUser = true;
      description = "Your Name";
      extraGroups = [ "wheel" ]; # for sudo access
    };
  - Add necessary packages:
    environment.systemPackages = with pkgs; [
      vim # or your preferred editor
    ];

## Connect to network
- Start wpa_supplicant service
```sh
sudo systemctl start wpa_supplicant
```
- Run wpa_cli
```sh
wpa_cli
```
- In the wpa_cli interface, enter these commands:
```
> add_network
0
> set_network 0 ssid "your_network_name"
OK
> set_network 0 psk "your_password"
OK
> enable_network 0
OK
```
- Wait for the connection to be established. You should see a message indicating successful connection4.
- Exit wpa_cli by typing quit.
```sh
quit
```

## Install NixOS
```sh
nixos-install
```


