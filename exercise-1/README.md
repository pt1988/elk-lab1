## Exercise 1 : Process web access log

#### 1.0. Preparation

##### 1.0.1. change privilege to root
```
sudo su -
```

##### 1.0.2. copy logs file to /var/log
```
cp access.log /var/log
```

#### 1.1. Run logstash with command line

show logstash path
```
whereis logstash
```

show logstash help
```
/usr/share/logstash/bin/logstash -h
```

run logstash 
```
/usr/share/logstash/bin/logstash -f logstash-stdout.conf
```

#### 1.2. Run logstash with systemd

copy to logstash config to logstash-systemd configuration directory
```
mkdir /etc/logstash/conf/
cp logstash-elasticsearch.conf /etc/logstash/conf/
```

restart logstash service
```
systemctl restart logstash
```

check logstash status
```
systemctl logstash status
```

