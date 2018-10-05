[[CentOS](README.md)]

[[Excerise1: logstash](exercise-1)]
[[Excerise2: elasticsearch](exercise-2)]
[[Excerise3: elasticsearch](exercise-3)]

### Exercise0 : Install ELK 

#### 1. Setup Environment. 

##### 1.0 Mapping domain 'artifacts.elastic.co' to intranet server (158.108.8.148)
```
sudo echo "158.108.8.148 artifacts.elastic.co" >> /etc/hosts
```

##### 1.1 Install prerequisite packets.

```
sudo apt install default-jre
sudo apt-get install apt-transport-https
```

##### 1.2 Add Elastic Public Signature key
```
sudo wget -qO - http://artifacts.elastic.co:8080/GPG-KEY-elasticsearch | sudo apt-key add 
```

##### 1.4 Add repository
```
sudo echo "deb http://artifacts.elastic.co:8080/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
```


#### 2. Install logstash
[[reference](https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)]

##### 2.1 Install logstash
```
sudo apt-get update && sudo apt-get install logstash -y
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
[[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html)]


##### 3.1 Install Elasticsearch
```
sudo apt install elasticsearch -y 
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

##### 3.3 test elasticsearch rest api
```
curl 127.0.0.1:9200
```

#### 4. Install Kibana
[[reference](https://www.elastic.co/guide/en/kibana/current/rpm.html)]


##### 4.1 install kibana
```
apt-get install kibana -y
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

#### 4.3 test kibana service
```
curl 127.0.0.1:5601
```

#### 5. Setup nginx revers proxy with basic authentication

##### 5.1) install nginx
[[reference](https://community.openhab.org/t/using-nginx-reverse-proxy-authentication-and-https/14542)]

```
sudo apt install -y nginx
```

##### 5.2) Add config

Open file "/etc/nginx/nginx.conf" and add reverse proxy config to session http { server { location / { [config] }}} 
```
sudo vim /etc/nginx/sites-available/default
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
sudo apt install apache2-utils
sudo htpasswd -c  /etc/nginx/.htpasswd admin
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

