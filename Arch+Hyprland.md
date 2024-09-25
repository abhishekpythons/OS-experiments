Steps
- make flash boot and boot from it
- connect to internet
  ```sh
  iwctl
  ```
- connect to your wifi or ethernet
  ```sh
  device list
  station <inteface name(wlan0)> scan
  station <interface name(wlan0)> get-netwrork
  station <interfacee name(wlan0)> connect <network name(IIST-STUDENT-WIFI)>
  # then enter password
  exit
  ```
- check network
  ```sh
  ip address
  ```
  this shoul be UP and a valid IP should be assiggned to your PC
