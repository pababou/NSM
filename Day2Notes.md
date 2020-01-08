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
  


# pfSense Instalation
ESC-enter BIOS setup

F11 -boot menu

## Initial Install
A accept

I-install (OK)

-default US keymap

Auto-Guided Disk Setup

....install runs...

reboot

## Install Config

### 1-) Assign Interfaces

|  LAN4  |  LAN3  | LAN2  |  LAN1  |  COM  |
|--------|--------|-------|--------|-------|
|  em3   |  em2   |  em1  |  em0   | local |


1-Assign interface

n-skip vlan setup

em0-WAN interface

em1-LAN interface

(nothing to finish) y-proceed


reloading.....

### 2) Set Interface IPs

1-WAN interface

y-dhcp

n-dhcp6

blank for "none"  y-http webconfig protocol

2-LLAN int

172.16.30.1/24 

<ENTER> for none

<ENTER> for none-disable ipv6

y-enbale DHCP

172.16.30.100-range start

172.16.30.254-range end

y-hhtp webconfig protocol


## Finshing Up (wizard)

in order to access the the router, complete the following

1. set you lab machine to static ip 172.16.30.10 

2. plug into the LAN interface (LAN on format face) and browse to 172.16.30.1

3. complete first login to web GUI

default creds:

admin / pfsense


## Wizard>Pfsense Setup > General Information


set hostname sg30 pfsense locationdomain==lacal.lan

primary dns

uncheck block private networks

LAN ip address -172.16.16.30.1

subnet mask-24

set (and document) new admin password


### log back in

dashboard overview

top rigth + and add

service status

traffic graph


### Firewall> rules


secure out the box default  deny behavior

firewall > rules > wan > add

pass, proto=all, source=any, dest=any

firewall > rules > lan > add

pass, proto=all, source=any, dest=any

now disable NAT firewall . nat > outbound: disable radio button

## Configure Remaining Interfaces

assign

interfaces > assignments

add em2 (OPT1)

add em3 (OPT2)

save


enable 

click interfaces name in list e.g OPT1

enable interface

ipv4 config type-none

ipv6 config type-none

save

check for double-gateway issue

system > routing:

make sure there is not more than one gateway entry



# Centos Networking


ifconfig depricated

ip replace

ip a (addr)

review information displayed

ip-4 addr - only show ipv4

UP, LOWER_UP, state Up

ip link set enp3847 down/up


## ip cheatsheet


ip addr

    ip a show dev enp0s3
    
    ip a add x.x.x.x/24 dev xxx
    
    ip a del x.x.x.x/24 dev xxx
    
    
 ip link
   
   ip link show dev enp0s3
   
   ip -s link
   
   ip link set dev xxx up
   
   ip link set dev xxx down
   
   ip link set empxxxx promisc on
   
ip route

ip neigh

  ip neigh show dev enp0s3
  


Manual Netork Setting


config file for each network interface:

ll /etc/sysconfig/network-scripts


## Use DHCP

edit the cofig file for desired interface:

sudo vi enp0s3

set BOOTPROTO=DHCP

remove ADDR / PREFIX / GATEWAY info (in any)

verify: restart networking and test


## Use Static


edit the config file for desired interface

sudo vi enp0s3


set BOOTPROTO=name

set IPADDR= <ip>

set PREFIX=24

set GATEWAY= <gwip>

set DNS1+<dnsip>

set DNS2 =<secondary>


verify: restart networking and test



## Disable IPv6

sysctl.conf

edit the config file to disable IPv6 on all interfaces

sudo vi /etc/sysctl.conf

net.ipv6.conf.all.disable_ipv6 = 1

net.ipv6.conf.default.disable_ipv6 = 1

net.ipv6.conf.lo.disable_ipv6 = 1

load change from /etc/sysctl.conf file 

sudo systctl -p

### host file

edit host file to remove the ipv6 entry ::1

sudo vi /etc/hosts


## Firewalld - Basic Usage


### open / allow traffic

sudo firewall-cmd --zone=public --add-port = ####/tcp --permanent

sudo firewall-cmd --reload


## Close/deny traffic


sudo firewall-cmd --zone=public --remove-port=####/tcp --permanent

sudo forewall-cmd --reload


### commit all running firewall rules into the startup rules

sudo firewall-cmd --runtime-to-permanent

### list all firewalld zones

list all firewalld-cmd --list-all-zones

### Monitoring Activity


what's a socket?
 
 ip / port / protocol defined to provide a connection or services
 
 ss    is the new hotness
 
 ss -lnt   shows all listening
 
 







    
 
 



















