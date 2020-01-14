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






