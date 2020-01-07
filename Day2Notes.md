# Connect to switch #
ssh perched@10.0.0.1

pwd perched1234!@#*$
## Enter privileged mode ##
enable

configure termial

## Setup dhcp ##
ip dhcp excluded-address 10.0.30.0 10.0.30.1

ip dhcp pool SG30

network 10.0.30.0 255.255.255.252

default-router 10.0.30.1

dns-server 192.168.2.1

exit

## Setup vlans

vlans 30

name SG30

state active

exit

## Add interfaces

interface GigabitEthernet 1/0/x

switchport

switchport access vlan SG30

no shutdown

exit

interface vlan no shu30

ip address 10.0.30.1 255.255.255.252

no shutdown

exit

## Enable static route

ip routing


ip route 172.16.30.0 255.255.255.0 10.0.30.2

exit
  













