# Filebeat

/usr/share/kafka/bin/kafka-topics.sh  --list --bootstrap-server 172.16.2.100:9092

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



 






