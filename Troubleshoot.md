# Troubleshout

ss -lnt

firewall-cmd --list-port

firewall-cmd --add-port=5601/port --permanent

firewall-cmd --remove-port=5601/port --permanent

firewall-cmd --reload


check topics



/usr/share/kafka/bin/kafka-console-consumer.sh --topic suricada-raw --bootstrap-server 172.16.30.102



