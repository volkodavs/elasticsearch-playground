# Elasticsearch Playground

## Overvierw 

This project starts 3 node elasticsearch cluster and load test dataset with Nobel laureates for playground purposes. 

## How to start 

```bash
docker-compose up
```


# Elasticsearch Mapping 

## Explicit mapping 

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

# Query examples 

### Nested query 

Search for the two Nobel laureates John Goodenough and  M. Stanley Whittingham

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
                                            "laureates.firstname": "M. Stanley"
                                        }
                                    },
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
