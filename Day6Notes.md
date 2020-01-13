# Filebeat
## Install and configure

/usr/share/kafka/bin/kafka-topics.sh  --list --bootstrap-server 172.16.30.102:9092

yum install filebeat

cd /etc/filebeat

vi filebeat.yml

replace the Filebeat input section with the following


#=========================== Filebeat inputs =============================
filebeat.inputs:
  - type: log
    paths:
      - /data/suricata/eve.json
    json.keys_under_root: true
    fields:
      kafka_topic: suricata-raw
    fields_under_root: true
  - type: log
    paths:
      - /data/fsf/logs/rockout.log
    json.keys_under_root: true
    fields:
      kafka_topic: fsf-raw
    fields_under_root: true
processors:
  - decode_json_fileds:
      fields:
        - message
        - Scan Time
        - Filename
        - objects
        - Source
        - meta
        - Alert
        - Summary
      process_array: true
      max_depth: 10
      
      
    Add the following under Outputs
      
      
      #================================ Outputs =====================================
    
     Configure what output to use when sending the data collected by the beat.
   
    141 #-----------------------------Kafka output------------------------------------
    142 output.kafka:
    143   hosts: ["172.16.30.102:9092"]
    144   topic: '%{[fields.kafka_topic]}'
    
    
    comment the Elasticsearch output in the same file
    

/opt/fsf/fsf-client/fsf_client.py /data/fsf/Bro-cheatsheet.pdf


## Install elasticsearch


yum install elasticsearch

vi /etc/elasticsearch/elasticsearch.yml


uncomment .....................



vi /etc/elasticsearch/jvm.options 

change the  the following 

#Xms represents the initial size of total heap space
#Xmx represents the maximum size of total heap space
     21 
     22 -Xms8g
     23 -Xmx8g
     
  

mkdir /etc/systemd/system/elasticsearch.service.d

vi  /etc/systemd/system/elasticsearch.service.d/overrides.conf


[Service]
LimitMEMLOCK=infinity


chown  -R elasticsearch: /data/elasticsearch
chown  -R elasticsearch: /etc/elasticsearch

firewall-cmd --zone=public --add-port=9200/tcp --permanent
firewall-cmd --zone=public --add-port=9300/tcp --permanent


curl localhost:9200

curl localhost:9200/_cat/nodes?v



## Install Kibana

yum install kibana



vi /etc/kibana/kibana.yml 


uncomment ...................

7 server.host: "172.16.30.102"

28 elasticsearch.hosts: ["http://localhost:9200"]

















 






