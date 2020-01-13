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
      
      
    A
    
    dd the following under Outputs
      
      
      #================================ Outputs =====================================
    
     Configure what output to use when sending the data collected by the beat.
   
    141 #-----------------------------Kafka output------------------------------------
    142 output.kafka:
    143   hosts: ["172.16.30.102:9092"]
    144   topic: '%{[fields.kafka_topic]}'
    
    
    comment the Elasticsearch output in the same file
    




 






