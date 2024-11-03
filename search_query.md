# overview 

the example bellow represents interaction with opensearch api:

- first of all, creating a index called students
- searching in student index 

## index creation 

curl -XPUT -u 'admin:q^wER4_@!' -k 'https://localhost:9200/students' -H 'Content-Type: application/json'

## bulk document creation

curl -XPOST -u 'admin:q^wER4_@!' -k 'https://localhost:9200/students/_bulk' --data-binary @students.json -H 'Content-Type: application/json'

# search 

base content: 

curl -X GET -u 'admin:q^wER4_@!' -k "https://localhost:9200/students/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { 
    "bool": { 
      "must": [
        { "match": { "name":   "alves" }},
        {"match": {"gender": "female"}}
      ],
      "filter": [ 
        { "range": { "age": { "gte": "20" }}}
      ]
    }
  }
}'
## search query match all
{ex
  "query": {
    "match_all": {}
  }
}'
## search query with body

{
  "query": {
    "match": {
      "name": "bob"
    }
  }
}'



## Boolean search query
    {
        "query": { 
            "bool": { 
            "must": [
                { "match": { "name":   "Bob" }},
            ],
            "filter": [ 
                { "range": { "age": { "gte": "30" }}}
                { "term":  { "status": "published" }},
            ]
            }
        }
    }

        
    - The query parameter indicates query context.

    - The bool and two match clauses are used in query context, which means that they are used to score how well each document matches.

    - The filter parameter indicates filter context. Its term and range clauses are used in filter context. They will filter out documents which do not match, but they will not affect the score for matching documents.