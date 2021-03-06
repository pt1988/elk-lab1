#### Exercise2 Elasticsearch

##### 1. REST API

###### 1.1) show elasticsearch information
```
curl -XGET 127.0.0.1:9200/
```

##### 2. CAT API
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat.html)

###### 2.1) show cat API function list
```
curl -XGET 127.0.0.1:9200/_cat
```

###### 2.2) show cluster health
```
curl 127.0.0.1:9200/_cat/health?v
```

###### 2.3) show node list in cluster 
```
curl 127.0.0.1:9200/_cat/nodes?v
```

###### 2.4) show indices list
```
curl 127.0.0.1:9200/_cat/indices?v
```

###### 2.5) show shades list
```
curl 127.0.0.1:9200/_cat/shards?v
```

###### 2.6) show template
```
curl 127.0.0.1:9200/_cat/templates?v
```

###### 2.6-1) show logstash template
```
curl 127.0.0.1:9200/_template/logstash?pretty=true
```


##### 3. Search API
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html)

search command pattern
```
curl 127.0.0.1:9200/[index_pattern]/[type_name]/_search
```

###### 3.1) search for all type
```
curl 127.0.0.1:9200/logstash*/_search?pretty=true
```

###### 3.2) search specific type
```
curl 127.0.0.1:9200/logstash*/doc,user/_search?pretty=true
```

###### 3.3) URI search 
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-uri-request.html)

```
curl 127.0.0.1:9200/logstash*/doc,user/_search?q=status:200
```

###### 3.4) Request Body Search 
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-body.html)

```
curl 127.0.0.1:9200/logstash*/_search?pretty=true --header "Content-Type: application/json" \
-d '{
    "query" : {
        "term" : { "status" : "200" }
    }
}'
```


##### 4. Aggregation Framework
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)


###### 4.1 Metric aggregation
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics.html)

This example show metric aggregation with [cadinality](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics-cardinality-aggregation.html). (Uniq Count)

```
curl 127.0.0.1:9200/logstash*/_search?pretty=true --header "Content-Type: application/json" \
-d '{
    "aggs" : {
	"type_count" : {
        	"cardinality" : { 
			"field" : "status.keyword"
		}
	}
    }
}'
```

###### 4.2) Bucket aggregation
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html)

This example show bucket aggregation using term.
```
curl 127.0.0.1:9200/logstash*/_search?pretty=true --header "Content-Type: application/json" \
-d '{
    "aggs" : {
	"logstash*" : {
        	"terms" : { 
			"field" : "status.keyword",
			"size" : 10
		}
	}
    }
}'
```


###### 4.3) Pipeline aggregation
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-pipeline.html)


###### 4.4) Matrix aggregation
[reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-matrix.html#search-aggregations-matrix)
 
