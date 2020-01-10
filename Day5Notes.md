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

vi json.zeek

add the following to the json.zeek file you just created

redef LogAscii::use_json = T;
redef LogAscii::json_timestamps = JSON::TS_ISO8601;



zeekctl check (to make sure everything is running fine)

zeekctl stop

zeekctl deploy

zeekctl cleanup all












 

 
 
 
 
 






