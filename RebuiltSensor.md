# Sensor Rebuild
    1  ll
    2  touch test.test
    3  sudo vi /etc/sysctl.conf 
    4  sudo sysctl -p
    5  vi /etc/hosts
    6  ll
    7  sudo firewall-cmd --zone=public --add-port = 8008/tcp --permanent
    8  sudo firewall-cmd --zone=public --add-port=8008/tcp --permanent
    9  sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
   10  sudo firewall-cmd --zone=public --add-port=9200/tcp --permanent
   11  sudo firewall-cmd --zone=public --add-port=9300/tcp --permanent
   12  sudo firewall-cmd --zone=public --add-port=5601/tcp --permanent
   13  sudo firewall-cmd --reload
   14  sudo firewall-cmd --runtime-to-permanent
   15  ss -lnt
   16  list all firewalld-cmd --list-all-zones
   17  firewalld-cmd --list-all-zones
   18  sudo firewalld-cmd --list-all-zones
   19  sudo firewall-cmd --list-all-zones
   20  clear
   21  pwd
   22  ll
   23  cp local-yum.repo /etc/yum.repos.d/.
   24  cd /etc/yum.repos.d/
   25  ll
   26  rm -rf CentOS-*
   27  ll
   28  pwd
   29  sudo yum install stenographer
   30  ll
   31  cd /etc/stenographer/
   32  ll
   33  cat config 
   34  ll
   35  cp config config_bk
   36  ll
   37  cp /root/steno-config config
   38  cat config
   39  ip a
   40  ll /data/steno/
   41  ll
   42  cd
   43  find -name 'stenokeys.sh'
   44  ll
   45  cd -
   46  ll
   47  cd /
   48  find -name 'stenokeys.sh'
   49  clear
   50  cd -
   51  ll
   52  ll certs/
   53  ll
   54  cd /etc/suricata/
   55  ll
   56  cp suricata.yaml_bk
   57  cp suricata.yaml suricata.yaml_bk
   58  ll
   59  cp /root/surricata.yaml suricata.yaml
   60  ll
   61  vi suricata.yaml
   62  suricata-update
   63  ll
   64  cd rules/
   65  ll
   66  suricata-update
   67  curl -L -O https://192.168.2.11:8009/suricata-5.0/emerging.rules.tar.gz
   68  ll
   69  cp /usr/share/suricata/classification.config  /etc/suricata/
   70  cp /usr/share/suricata/reference.config  /etc/suricata/.
   71  cd ..
   72  ll
   73  sudo firewall-cmd --zone=public --add-port=8009/tcp --permanent
   74  curl -L -O https://192.168.2.11:8009/suricata-5.0/emerging.rules.tar.gz
   75  sudo systemctl start stenographer
   76  sudo systemctl status stenographer
   77  sudo systemctl status suricata
   78  sudo systemctl start suricata
   79  sudo systemctl status suricata
   80  ll
   81  ll rules/
   82  ll
   83  sudo cat /proc/cpuinfo | egrep -e 'processor|physical id|core id' | xargs -l3 
   84  curl -L -O http://192.168.2.11:8009/suricata-5.0/emerging.rules.tar.gz
   85  ll
   86  rm emerging.rules.tar.gz 
   87  ll
   88  cd rules/
   89  ll
   90  curl -L -O http://192.168.2.11:8009/suricata-5.0/emerging.rules.tar.gz
   91  ll
   92  chown suricata: emerging.rules.tar.gz 
   93  ll
   94  sudo systemctl start suricata
   95  sudo systemctl status suricata
   96  ll /data/suricata/
   97  chown -R /data/suricata/
   98  chown -R suricata: /data/suricata/
   99  ll /data/suricata/
  100  sudo systemctl start suricata
  101  sudo systemctl status suricata
  102  ll
  103  cd ..
  104  ll
  105  vi suricata.yaml
  106  cp suricata.yaml_bk2
  107  cp suricata.yaml suricata.yaml_bk2
  108  ll
  109  vi suricata.yaml
  110  sudo systemctl start suricata
  111  sudo systemctl status suricata
  112  ll
  113  cp suricata.yaml_bk2 suricata.yaml
  114  ll
  115  vi suricata.yaml
  116   vi /etc/sysconfig/suricata
  117  sudo systemctl start suricata
  118  sudo systemctl status suricata
  119   vi /etc/sysconfig/suricata
  120  sudo systemctl start suricata
  121  sudo systemctl status suricata
  122  history
  123  cd /root
  124  ll
  125  history > history


