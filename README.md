[lexcerise1: ogstash](exercise-1).

[eexcerise2: lasticsearch](exercise-2).

### Exercise0 : Install ELK 

#### 1. Install Java
```
yum install java -y
```



#### 2. Install logstash

##### 2.0.1 Add Public Signature key
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

##### 2.1 Add repository
```
echo '
[logstash-6.x] 
name=Elastic repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
' > /etc/yum.repos.d/elasticsearch.repo
```

##### 2.2 Install logstash
```
sudo yum install logstash
```

##### 2.3 Start logstash service
```
sudo systemctl start logstash
sudo systemctl status logstash
sudo systemctl enable logstash 
```

#### 3. Install Elasticsearch

##### 3.1 Add Elasticsearch Repository 
```
echo '
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
' > /etc/yum.repos.d/elasticsearch.repo 
```

##### 3.2 Install Elasticsearch
```
sudo yum -y install elasticsearch
```

##### 3.3 Start elasticsearch service
```
#start service
sudo systemctl start elasticsearch 

#check service status
sudo systemctl status elasticsearch

#enable elasticsearch start on boot
sudo systemctl enable elasticsearch
```

####4. Install Kibana

##### 4.1 Add kibana repository
```
echo '
[kibana-6.x]
name=Kibana repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
' > /etc/yum.repos.d/kibana.repo
```

##### 4.2 edit config
###### edit server host
open file /etc/kibana/kibana.yml 
```
vim /etc/kibana/kibana.yml 
```
replace line server.host parameter from localhost to 0.0.0.0
```
server.host : 0.0.0.0 
``` 

##### 4.3 start kibana service
```
# start kibana
sudo systemctl start kibana

# check kibana service status
sudo systemctl status kibana

# enable kibana start onboot
sudo systemctl enable kibana 
```

##### 4.4 config firewall to open tcp port 5601 for kibana
```
firewall-cmd --add-port 5601/tcp --zone=public --permanent
firewall-cmd --reload
```
