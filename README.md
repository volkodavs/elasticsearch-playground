# Elasticsearch Playground

## Overvierw 

This project starts 3 node elasticsearch cluster with Nobel laureates dataset. 

```json 
{
  "year": "2019",
  "category": "chemistry",
  "laureates": [
    {
      "id": "976",
      "firstname": "John",
      "surname": "Goodenough",
      "motivation": "for the development of lithium-ion batteries",
      "share": "3"
    },
    
    ...
    
    ]
 }
```

## How to start 

```bash
docker-compose up
```
**NOTE** 
By default, a fresh new cluster created

# Elasticsearch Mapping 

## Nobel mapping 

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
