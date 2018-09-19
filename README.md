### Exercise1 : Install ELK
#### 1. Install Java
```
yum install java -y
```

#### 2. Install logstash
```
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
yum -y install elasticsearch
```

##### 3.3 Start elasticsearch service
```
#start service
systemctl start elasticsearch 

#check service status
systemctl status elasticsearch

#start elasticsearch on boot
systemctl enable elasticsearch
```

4. Install Kibana
```
```
### 
