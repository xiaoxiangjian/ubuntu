> ironic env, BM bond to MLAG Leaf 

##### 1 check ifenslave package
```shell
# dpkg -l | grep ifenslave
```
##### 2 load bonding module
```shell
# modprobe bonding
```
##### 3 set auto load bonding module on startup
```shell
# echo bonding >>/etc/modules
```

##### 4 create bond
```shell
# cat /etc/network/interfaces

  auto ens2f0
  iface ens2f0 inet manual
      bond-master bond0
  auto ens2f1
  iface ens2f1 inet manual
      bond-master bond0

  auto bond0
  iface bond0 inet dhcp
      pre-up ifconfig bond0 hw ether xx:xx:xx:xx:xx:xx # same with ironic port-group OR vport mac on SE-DC
  #   address 192.168.1.10
  #   gateway 192.168.1.1
  #   netmask 255.255.255.0
      bond-mode 4
      bond-miimon 100
      bond-lacp-rate 1 # lacp short period,flags on device side: ABCDEF
  #   bond-lacp-rate 1 # comment,lacp long period,flags on device side: ACDEF
      bond-slaves ens2f0 ens2f1
  iface bond0 inet6 dhcp
      
```
##### 5 reboot host
```shell
# reboot
```
