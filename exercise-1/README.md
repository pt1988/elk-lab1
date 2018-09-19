## Exercise 1 : Process web access log

#### 1.0. Preparation

##### 1.0.1. git clone project from gitlab
```
git clone https://gitlab.com/pt1988/elk-lab1.git
cd elk-lab1/exercise-1
```

##### 1.0.2. change privilege to root
```
sudo su -
```

##### 1.0.3. copy logs file to /var/log
```
cp access.log /var/log
```

#### 1.1. Run logstash with command line
Using logstash to read logfile(/var/log/access.log) and process and print to screen.

Show logstash path
```
whereis logstash
```

Show logstash help
```
/usr/share/logstash/bin/logstash -h
```

Run logstash 
```
/usr/share/logstash/bin/logstash -f logstash-stdout.conf
```

#### 1.2. Run logstash with systemd 
Using logstash to read logfile(/var/log/access.log) to process and store to Elasticsearch.

Copy to logstash config to logstash-systemd configuration directory
```
mkdir /etc/logstash/conf.d/
cp logstash-elasticsearch.conf /etc/logstash/conf.d/
```

Restart logstash service
```
systemctl restart logstash
```

Check logstash status
```
systemctl logstash status
```

Troubleshooting logstash
```
tail /var/log/logstash/logstash-plain.log -f
```
