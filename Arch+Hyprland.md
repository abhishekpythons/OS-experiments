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
```sh
cfdisk </dev/disk_name(/dev/nvmeon1)>
# use arrows keys to make partitions as your choice
# [Write] to save the partition table
# [Quit] to get back to the linux prompt
```
again check using ```lsblk```
