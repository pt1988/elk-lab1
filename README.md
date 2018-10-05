[[Install ELK on Ubuntu](UBUNTU.md)]

[[Excerise1: logstash](exercise-1)]
[[Excerise2: elasticsearch](exercise-2)]
[[Excerise3: elasticsearch](exercise-3)]

## Install ELK on CentOS7

### Exercise0 : Install ELK 

#### 1. Setup Environment. 

##### 1.0 Mapping domain 'artifacts.elastic.co' to intranet server (158.108.8.148)
```
sudo echo "158.108.8.148 artifacts.elastic.co" >> /etc/hosts
```

##### 1.1 Disable SE Linux

##### 1.2 Install Java.

```
yum install java -y
```

##### 1.3 Add Elastic Public Signature key
```
rpm --import http://artifacts.elastic.co:8080/GPG-KEY-elasticsearch
```

##### 1.4 Add repository
```
echo '
[elk-6.x] 
name=Elastic repository for 6.x packages
baseurl=http://artifacts.elastic.co:8080/packages/6.x/yum
gpgcheck=1
gpgkey=http://artifacts.elastic.co:8080/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
' > /etc/yum.repos.d/elastic.repo
```


#### 2. Install logstash
[[reference](https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)]

##### 2.1 Install logstash
```
sudo yum install logstash -y
```

##### 2.2 Start logstash service
```
#start logstash serivce with systemd 
sudo systemctl start logstash

#check logstash service status
sudo systemctl status logstash

#enable logstash auto start onboot
sudo systemctl enable logstash 
```


#### 3. Install Elasticsearch
[[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/rpm.html)]


##### 3.1 Install Elasticsearch
```
sudo yum -y install elasticsearch -y 
```

##### 3.2 Start elasticsearch service
```
#start service
sudo systemctl start elasticsearch 

#check service status
sudo systemctl status elasticsearch

#enable elasticsearch start on boot
sudo systemctl enable elasticsearch
```

##### 3.3 test elasticsearch's http rest api
```
curl 127.0.0.1:9200
```

#### 4. Install Kibana
[[reference](https://www.elastic.co/guide/en/kibana/current/rpm.html)]


##### 4.1 install kibana
```
yum install kibana -y
```

##### 4.2 start kibana service
```
# start kibana
sudo systemctl start kibana

# check kibana service status
sudo systemctl status kibana

# enable kibana start onboot
sudo systemctl enable kibana 
```

#### 4.3 test kibana's http service
```
curl 127.0.0.1:5601
```

#### 5. Setup nginx revers proxy with basic authentication

##### 5.1) install nginx
[[reference](https://community.openhab.org/t/using-nginx-reverse-proxy-authentication-and-https/14542)]

```
yum install -y epel-release
yum install -y nginx
```

##### 5.2) Add config

Open file "/etc/nginx/nginx.conf" and add reverse proxy config to session http { server { location / { [config] }}} 
```
vim /etc/nginx/nginx.conf
```

Copy this configuration place in file nginx.conf
```
location / {
		proxy_pass                            http://localhost:5601/;
		proxy_buffering                       off;
		proxy_set_header Host                 $http_host;
		proxy_set_header X-Real-IP            $remote_addr;
		proxy_set_header X-Forwarded-For      $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Proto    $scheme;
                auth_basic                            "Username and Password Required";
                auth_basic_user_file                  /etc/nginx/.htpasswd;
}
```

##### 5.3) Create username and password for http basic authentication

```
sudo yum install -y httpd-tools
htpasswd -c /etc/nginx/.htpasswd admin
```

##### 5.4 start kibana service
```
# start nginx service
sudo systemctl start nginx

# check nginx service status
sudo systemctl status nginx

# enable nginx start onboot
sudo systemctl enable nginx
```

##### 5.4) config firewall to allow port http 80
```
firewall-cmd --add-port 80/tcp --zone=public --permanent
firewall-cmd --reload
```

