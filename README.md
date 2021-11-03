# Learn ElasticSearch Cluster

## Setup
1. Install docker and docker-compose
2. Install postman
3. Run command: docker-compose up -d
4. Import postman collection to postman

---

## Info
- Minimum (N/2)+1 nodes to start the cluster.
- If we have 3 nodes, at least 2 nodes must run on startup

---

## API

### Check Node Port
```
http://localhost:9201
```
![Image](/images/escluster-default_port.png)


### Get Cluster's Node Info
```
GET http://localhost:9201/_cat/nodes?v
```
![Image](/images/escluster-get_node_info.png)

### Create Index
```
PUT http://localhost:9202/learn_es_index
```
![Image](/images/escluster-create_index.png)

### Get Detailed Index
- Index information will exist across nodes in the cluster
```
GET http://localhost:9203/learn_es_index
```
![Image](/images/escluster-get_detail_index.png)

### Get Indices Info in Cluster
```
http://localhost:9203/_cat/indices?v
```
![Image](/images/escluster-get_indices_info_in_cluster.png)

### Get Shards Info in Cluster (Document Distribution)
```
http://localhost:9203/_cat/shards?v
```
![Image](/images/escluster-shards_info_in_cluster.png)


## Performing CRUD operations
### Create Document - PUT
```
PUT http://localhost:9203/learn_es_index/_doc/1
{
    "programming_language": "java",
    "typing_type": "static"
}
```
![Image](/images/escluster-create_doc_put.png)

```
PUT http://localhost:9203/learn_es_index/_doc/2
{
    "programming_language": "golang",
    "typing_type": "static"
}
```
![Image](/images/escluster-create_doc_put_2.png)

```
PUT http://localhost:9203/learn_es_index/_doc/3
{
    "programming_language": "kotlin",
    "typing_type": "static"
}
```
![Image](/images/escluster-create_doc_put_3.png)


### Create Document - POST (auto increment)
```
POST http://localhost:9203/learn_es_index/_doc
{
    "programming_language": "python",
    "typing_type": "dynamic"
}
```
![Image](/images/escluster-create_doc_post.png)

### Read Document
```
GET http://localhost:9203/learn_es_index/_doc/3
```
![Image](/images/escluster-get_doc_3.png)

```
GET http://localhost:9203/learn_es_index/_doc/dummy_id 
```
![Image](/images/escluster-get_doc_not_found.png)


### Update Document
```
PUT http://localhost:9203/learn_es_index/_doc/1
{
    "programming_language": "swift",
    "typing_type": "static"
}
```
![Image](/images/escluster-update_doc.png)

### Delete Document
```
DELETE http://localhost:9203/learn_es_index/_doc/KVdu43wBXN6h2Uu3uG1t
```
![Image](/images/escluster-delete_doc.png)

---

## Performing Search operations
### Search All Index
```
GET http://localhost:9203/_search
```
![Image](/images/escluster-search_all_index.png)

### Search Specific Index
```
GET http://localhost:9203/learn_es_index/_search
```
![Image](/images/escluster-search_specific_index.png)

### Search Query Match
```
GET http://localhost:9203/learn_es_index/_search
{
    "query": {
        "match": {
            "typing_type": "dynamic"
        }
    }
}
```
![Image](/images/escluster-search_query_match.png)

### Search Bool Query (must)
```
GET http://localhost:9203/learn_es_index/_search
{
    "query": {
        "bool": {
            "must": {
                "match": {
                    "typing_type": "static"
                }
            }
        }
    }
}
```
![Image](/images/escluster-search_bool_query.png)

### Search Bool Query (should)
```
GET http://localhost:9203/learn_es_index/_search
{
    "query": {
        "bool": {
            "must": [],
            "must_not": [],
            "should": [
                {
                    "match": {
                        "programming_language": "golang"
                    }
                },
                {
                    "match": {
                        "programming_language": "kotlin"
                    }
                }
            ]
        }
    }
}
```
![Image](/images/escluster-search_bool_must.png)

### Search Aggregation
```
http://localhost:9203/learn_es_index/_search
{
    "query": {
        "bool": {
            "must": []
        }
    },
    "aggs": {
        "result_typing_type": {
            "terms": {
                "field": "typing_type.keyword"
            }
        },
        "result_programming_lang": {
            "terms": {
                "field": "programming_language.keyword"
            }
        }
    }
}
```
![Image](/images/escluster-search_aggregation.png)