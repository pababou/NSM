# Troubleshout

ss -lnt

firewall-cmd --list-port

firewall-cmd --add-port=5601/port --permanent

firewall-cmd --remove-port=5601/port --permanent

firewall-cmd --reload


check topics


/usr/share/kafka/bin/kafka-topics.sh --list --bootstrap-server 172.16.30.102:9092


/usr/share/kafka/bin/kafka-console-consumer.sh --topic suricada-raw --bootstrap-server 172.16.30.102  (check for data)


tcpdump  -i  enp2s0

ip link set enp2s0 up

curl 172.16.30.102:9200/_cat/indices  (make sure elasticsearch works)


 



yum install git

cd /etc/

get init

 git add .
 
 git commit -m "working state"
 
 git status
 
 get diff






