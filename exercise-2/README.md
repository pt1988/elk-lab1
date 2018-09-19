#### Exercise2 Elasticsearch

##### 1 REST API

###### 1.1 show elasticsearch information
```
curl -XGET 127.0.0.1:9200/
```

###### 1.2 CAT API
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat.html)

1.2.1) show cat API function list
```
curl -XGET 127.0.0.1:9200/_cat
```

1.2.2) show cluster health
```
curl 127.0.0.1:9200/_cat/health?v
```

1.2.3) show node list in cluster 
```
curl 127.0.0.1:9200/_cat/nodes?v
```

1.2.4) show indices list
```
curl 127.0.0.1:9200/_cat/indices?v
```

1.2.5) show shades list
```
curl 127.0.0.1:9200/_cat/shards?v
```

1.2.6) show template
```
curl 127.0.0.1:9200/_cat/templates?v
```

1.2.6-1) show logstash template
```
curl 127.0.0.1:9200/_template/logstash?pretty=true
```


###### 1.3 Search API
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html)

search command pattern
```
curl 127.0.0.1:9200/[index_pattern]/[type_name]/_search
```

1.3.1) search for all type
```
curl 127.0.0.1:9200/logstash*/_search?pretty=true
```

1.3.2) search specific type
```
curl 127.0.0.1:9200/logstash*/doc,user/_search?pretty=true
```

1.3.3) URI search 
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-uri-request.html)

```
curl 127.0.0.1:9200/logstash*/doc,user/_search?q=status:200
```

1.3.4) Request Body Search 
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-body.html)

```
curl 127.0.0.1:9200/logstash*/_search?pretty=true --header "Content-Type: application/json" \
-d '{
    "query" : {
        "term" : { "status" : "200" }
    }
}'
```


