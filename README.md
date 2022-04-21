# ELK Sandbox

## Description

This project consists of 4 containers:

 * Elasticsearch
 * Kibana
 * Logstash
 * Logger (running [log-generator](https://pypi.org/project/log-generator/) and filebeat)

## Running the ELK stack

### 1. Start the containers

```
docker-compose up -d
```

### 2. Use the Elasticsearch API to update the index settings

This reduces the replica count to zero so that the test index will be green.

```
curl -XPUT http://localhost:9200/logger_apache_access_logs/_settings \
  -H "Content-Type: application/json" \
  -d "{\"number_of_replicas\":\"0\"}"
```

### 3. Use the Kibana API to add the index pattern

```
curl -X POST http://localhost:5601/api/saved_objects/index-pattern \
  -H "Content-Type: application/json" \
  -H "kbn-xsrf: reporting" \
  -d "{\"attributes\":{\"title\":\"logger*\",\"timeFieldName\":\"@timestamp\"}}"
```

### 4. Visit Kibana in browser

[http://localhost:5601](http://localhost:5601)

### 5. Confirm that events are visible using Discover
