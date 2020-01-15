# Day 7

## Install and configure Logstash

yum install logstash

vi /etc/logstash/logstash.yml   (no change in this file)

cd /etc/logstash/conf.d/


## pipeline configuration 


vi 100-input-zeek.conf  (add the following)


input {
  kafka {
    add_field => { "[@metadata][stage] => "zeek-raw" }
    topics => ["zeek-raw"] 
    bootstrap_servers => "172.16.30,102:9092"
    #consumer_threads => 4
    group_id => "bro_logstash"
    auto_offset_reset =>"earliest"

  }
}

vi 500-filter-zeek.conf  (add the following)

filter{

....add.....

}


vi 999-output-zeek.conf  ( add the following)


output {
  if [@metadata][stage] == "zeek-raw" {
    elasticsearch {
      hosts => ["172.16.30.102:9200"]
      index => "zeek-%{+yyyy.MM.dd}"
      template => "/etc/logstash/bro-index-template.json"


   
  }
 }
}


curl 172.16.30.102:9200/_cat/indices


### troubleshooting


/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/100-input-zeek.conf -t



vi /etc/logstash/bro-index-template.json (open kibana and copy and past the mapping on this file)



copy it to the kibana dev tool



vi 500-filter-zeek.conf  (add the following)

filter{
   if [@metadata][stage] == "zeek-raw" {
      mutate {
           add_field => { "processed_time" => "@timestamp"}
      }
      date { match => ["ts", "ISO8601" ] }
      mutate {
        add_field => {"orig_host" => "%{id.orig_h}"}
        add_field => {"resp_host" => "%{id.resp_h}"}
        add_field => {"src_ip" => "%{id.orig_h}"}
        add_field => {"dst_ip" => "%{id.resp_h}"}
        add_field => {"related_ips" => []}
      }
      mutate {
        merge => {"related_ips" => "id.orig_h"}
      }
      
      mutate {
        merge => {"related_ips" => "id.resp_h"}
     }
   }
}


/usr/share/kafka/bin/kafka-topics.sh --list --bootstrap-server 172.16.30.102:9092


cd ~

systemctl stop logstash suricata stenographer fsf kafka zookeeper elasticsearch

rm -rf /var/lib/zookeeper/version-2/

rm -rf /data/kafka/*

rm -f /data/fsf/logs/rockout.log

rm -f /data/suricata/eve.json

rm -f /data/steno/thread0/packets /index/ packets/

rm -f /data/steno/thread0/packets/*

rm -f /data/steno/thread0/index/*

systemctl start elasticsearch

systemctl start suricata stenographer fsf kafka zookeeper











