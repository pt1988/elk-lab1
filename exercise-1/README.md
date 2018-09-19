## Exercise 1 : Process web access log with logstash

#### 1.0. Setup environment

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

1.1.1) Show logstash path
```
whereis logstash
```

1.1.2) Show logstash help
```
/usr/share/logstash/bin/logstash -h
```

1.1.3) Run logstash 
```
/usr/share/logstash/bin/logstash -f logstash-stdout.conf
```

#### 1.2. Run logstash with systemd 
Using logstash to read logfile(/var/log/access.log) to process and store to Elasticsearch.

1.2.1) Copy to logstash config to logstash-systemd configuration directory
```
mkdir /etc/logstash/conf.d/
cp logstash-elasticsearch.conf /etc/logstash/conf.d/
```

1.2.2) Restart logstash service
```
systemctl restart logstash
```

1.2.3) Check logstash status
```
systemctl logstash status
```

1.2.4) Troubleshooting logstash
```
tail /var/log/logstash/logstash-plain.log -f
```
