# Install Bro/Zeek

new version

zeek

zeekctl

zeek-core

zeek-plugin-af_packet

zeek-plugin-kafka

   yum install zeek zeek-plugin-af_packet zeek-plugin-kafka


for Bro ( just replcae zeek by bro)

### Baseline Configuration

location of the config file depends on what RPM  you install

   in /etc/zeek as wher most   others places under  /opt/zeek/etc

network config file

   networks.cfg
   
used to tell zeek about the local trusted IP space using CIDR notation
Has private IP range by default

  zeekctl.cfg
  
  LogDir = /data/zeek
  
  log-dir; the directory zeek writes its logs
  
  mkdir /data/zeek
  
open networks.cfg  and add your ip (192.168.30.0/24 sg30)


add the following line   to /etc/fstab 

  vi /etc/fstab 

`/home/srv /srv   none    bind            0 0



in the file zeekctl.cfg  add the folling 

lb_custom.InterfacePrefix=af_packet:: Note: this line does not exist by default and need to be added in order to tell zeek to use af_packet


modify the node.cfg file 

node.cfg
 
uncomment the [loger], [ manager], [proxy], [worker]
 
 
[logger]
type=logger
host=localhost

[manager]
type=manager
host=localhost
pin_cpus=3

[proxy-1]
type=proxy
host=localhost

[interface-enp2s0]
type=worker
host=localhost
interface=enp2s0
lb_method=custom
lb_procs=2
pin_cpus=1,2
env_vars=fanout_id=99

#[worker-]
#type=worker
#host=localhost
#interface=eth0


 cd /usr/share/zeek/site/
 
 vi local.zeek
 
 uncomment the last three load policy
 
 add the following also
 
#@load scripts/json
@load scripts/afpacket
@load scripts/kafka

 mkdir scripts
 
 cd scripts/
 
vi afpacket.zeek

add the following line to the afpacket.zeek file you just created

redef AF_Packet::fanout_id = strcmp(getenv("fanout_id"), "") == 0 ? 0 : to_count(getenv("fanout_id"));


vi kafka.zeek

add the following to the kafka.zeek file you just created


redef Kafka::topic_name = "zeek-raw";
redef Kafka::json_timestamps = JSON::TS_ISO8601;
redef Kafka::tag_json = F;
redef Kafka::kafka_conf = table(
     ["metadata.broker.list"] = "172.16.2.100:9092"
);

#Enable bro logging to kafka for all logs
event bro_init() &priority=-5
{
    for (stream_id in Log::active_streams)
    {
        if (|Kafka::logs_to_send| == 0 || stream_id in Kafka::logs_to_send)
        {
            local filter: Log::Filter = [
                $name = fmt("kafka-%s", stream_id),
                $writer = Log::WRITER_KAFKAWRITER,
                $config = table(["stream_id"] = fmt("%s", stream_id))
            ];

            Log::add_filter(stream_id, filter);
        }
    }
}



vi json.zeek

add the following to the json.zeek file you just created

redef LogAscii::use_json = T;
redef LogAscii::json_timestamps = JSON::TS_ISO8601;



zeekctl check (to make sure everything is running fine)

zeekctl stop

zeekctl deploy

zeekctl cleanup all

systemctl status zeek (to make sure zeek is running)


uncomment #@load scripts/json on the local.zeek   located cd /usr/share/zeek/site/



## Install fsf

yum install fsf


## configure fsf

cd /opt/fsf/


vi /opt/fsf/fsf-server/conf/config.py

#!/usr/bin/env python
#Basic configuration attributes for scanner. Used as default
#unless the user overrides them.

import socket

SCANNER_CONFIG = { 'LOG_PATH' : '/data/fsf/logs',
                   'YARA_PATH' : '/var/lib/yara-rules/rules.yara',
                   'PID_PATH' : '/run/fsf/scanner.pid',
                   'EXPORT_PATH' : '/data/fsf/files',
                   'TIMEOUT' : 60,
                   'MAX_DEPTH' : 10,
                   'ACTIVE_LOGGING_MODULES' : ['scan_log', 'rockout'],
                   }

SERVER_CONFIG = { 'IP_ADDRESS' : "172.16.30.102",
                  'PORT' : 5800 }




vi /opt/fsf/fsf-client/conf/config.py


#!/usr/bin/env python
#Basic configuration attributes for scanner client.
#'IP Address' is a list. It can contain one element, or more.
#If you put multiple FSF servers in, the one your client chooses will
#be done at random. A rudimentary way to distribute tasks.

SERVER_CONFIG = { 'IP_ADDRESS' : ['172.16.30.102',],
                  'PORT' : 5800 }

#Full path to debug file if run with --suppress-report
CLIENT_CONFIG = { 'LOG_FILE' : '/tmp/client_dbg.log' }



mkdir /data/fsf/log/{logs,files}

chown -R fsf: /data/fsf/

systemctl start fsf

systemctl status fsf

ss -lnt 


firewall-cmd --add-port=5800/tcp --permanent

firewall-cmd --reload

curl -L -O http://192.168.2.11:8009/Bro-cheatsheet.pdf


/opt/fsf/fsf-client/fsf_client.py --full Bro-cheatsheet.pdf



## Kafka
Overview 

     topic

    partitions

    replication

Install 

    Zookeeper
 
    kafka Node
    
Configure

    standalone
    
    cluster (entire class)
    

Download tar ball from official mirrors


curl -L -O

untar and move to /opt directory

tar -xzf kafka*


yum install zookeeper kafka


vi /etc/zookeeper/zoo.cfg   (no change for now)


systemctl start zookeeper

systemctl status zookeeper

systemctl enable zookeeper


vi /etc/kafka/server.properties (uncomment and add ip  on line 30 and 36)


ystemctl start kafka

systemctl status kafka































 

 
 
 
 
 






