# Day4
## Install Google stenographer

    sudo yum unstall stenographer
    
    cd /etc/stenographer

    vi config

    which stenographer (localtion)

(change "PacketsDirectory": to the: to the data/steno/thread0....... we make when installing centos)

(change "interface": enp2s0 ( inet ........)

    stenokeys.sh stenographer stenographer 

    ll /data/
    
    chown -R stenographer: /data/steno/
    
    systemctl start stenographer 
    
    systemctl status stenographer (make sure stenographer is running)
    
###     Troubleshooting step

    journalctl -xew stenographer (look for exit)
    
    ll /etc/stenographer/certs (look for certificate gernerated, will be empty if no cert)
    
    
    
  Stop steno
    
    systemctl stop stenographer 
    
##     ethtool
    
    
  Manually run ethtool
    
    ethtool -K enp2s0 tso off gro  off lro off  gso off rx off tx  off sg off rxvlan off txvlan off
    
    ethtool -N enp2s0 rx-flow-hash udp4 sdfn
    
    ethtool -N enp2s0 rx-flow-hash udp6 sdfn
    
    ethtool -C enp2s0 adaptive-rx off
    
    ethtool -C  enp2s0 rx-usecs 1000
    
    ethtool -G enp2s0 rx 4096
    
    
    
    
    script version
    
    #!/bin/bash
    
    
    for var in $@
    do
    
    echo "turning off affloading on $var"
    
    ethtool -K  $var tso off gro  off lro off  gso off rx off tx  off sg off rxvlan off txvlan off
    
    ethtool -N  $var rx-flow-hash udp4 sdfn
    
    ethtool -N  $var rx-flow-hash udp6 sdfn
    
    ethtool -C $var adaptive-rx off
    
    ethtool -C $var rx-usecs 1000
    
    ethtool -G $var rx 4096
    
    done
    
    exit 0
    
    
##     Install Suricata
    
    
    yum install suricata
    
    yum install tcpdump
    
    tcpdump -i enp2s0
    
    vi /etc/suricata/suricata.yaml
    
    :set nu 
    
    
    line 76   enabled: no
    
    line 404 enabled: no
    
    
    /enabled:yes (for search in vi)
    
    default-log-dir  ( change to location)
    
    outputs
    
    rule-files
    
    
    cd /etc/suricata 
   
    
    cd /var/lib/suricata/rules
    
    suricata-update 
   
    usr/share/local/suricata/rules
    
    /var/lib/suricata/rules
    
    vi /etc/sysconfig/suricata
    
    
    sudo cat /proc/cpuinfo | egrep -e 'processor|physical id|core id' | xargs -l3 
    
    
    
    cd /usr/share/suricata/rules/  ( copy link from share site )
    
    curl -L -O https://192.168.2.11:8009/suricata-5.0/emerging.rules.tar.gz
    
    ls 
    
    
    
    
    
    cp /usr/share/suricata/classification.config  /etc/suricata/.
    
    
    cp /usr/share/suricata/reference.config  /etc/suricata/.

    
    
    
    
    
    
    
    
    
    
    
    
    
    
   


    
    
    






