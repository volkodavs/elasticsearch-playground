# Elasticsearch Playground

## Overview 

### Mapping template 

```json
{
  "mappings": {
    "properties": {
      "year": { "type": "integer"},
      "category": {"type": "text"},
      "laureates": {
        "type": "nested", 
        "properties" : {
          "id": {"type": "long"},
          "firstname": {"type": "text"},
          "surname": {"type": "text"},
          "motivation": {"type": "text"},
          "share": {"type": "text"}
        }
      }
    }
  }
}
```

## Configuration 

3 elasticsearch data nodes 
1 elastic-importer

## Installation 

```bash
docker-compose up
```


# Query examples 

### Nested query 

Search for the two Nobel laureates John  Goodenough and  M. Stanley Whittingham

```bash 

curl --location --request GET 'http://localhost:9200/nobel/_search' \
--header 'Content-Type: application/json' \
--data-raw '{
    "query": {
        "bool": {
            "must": [
                {
                    "nested": {
                        "path": "laureates",
                        "query": {
                            "bool": {
                                "must": [
                                    {
                                        "match": {
                                            "laureates.firstname": "John"
                                        }
                                    },
                                    {
                                        "match": {
                                            "laureates.surname": "Goodenough"
                                        }
                                    }
                                ]
                            }
                        }
                    }
                },
                {
                    "nested": {
                        "path": "laureates",
                        "query": {
                            "bool": {
                                "must": [
                                    {
                                        "match": {
                                            "laureates.surname": "Whittingham"
                                        }
                                    }
                                ]
                            }
                        }
                    }
                }
            ]
        }
    }
}'
```
