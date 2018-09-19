## Exercise 1 : Process web access log

#### 1.0 : initiation

##### 1.0.1 : change privilege to root
```
sudo su -
```

##### 1.0.2 : copy logs file to /var/log
```
cp access.log /var/log
```

#### 1.1 : Run logstash with command line

show logstash directory
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

#### 1.2 : Run logstash with systemd





