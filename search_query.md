
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