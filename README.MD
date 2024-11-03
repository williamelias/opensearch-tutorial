##

## main concepts

- document: is object stored in json format
- index: is a collection of documents
- nodes: serves that process your requests
- cluster: collection of nodes
- shards: splits of nodes


## testing instalation


- curl https://localhost:9200 -ku admin:q^wER4_@!


## configuration

https://opensearch.org/docs/latest/install-and-configure/install-opensearch/index/


## Third party packages

https://www.elastic.co/guide/en/logstash/8.15/installing-logstash.html#_apt

### install jdbc plugin to connect with postgres and others databases

sudo /usr/share/logstash/bin/logstash-plugin install logstash-input-jdbc

### Download jdbc

https://jdbc.postgresql.org/download/

put it in /usr/share/logstash/logstash-core/lib/jars


### Configure logstash.yml

in /etc/logstash/logstash.yml add 'config.support_escapes: true'


### create a conf file in /etc/logstash/conf.d named as usersync.conf, naming is up to you.

    input {
        jdbc {
            jdbc_connection_string =>     “jdbc:postgresql://localhost:5432/testdatabase”
            jdbc_user => “db_user”
            jdbc_password => “db_password”
            jdbc_driver_class => “org.postgresql.Driver”
            statement => “SELECT * from public.\”Users\””
        }
    }
    output {
        elasticsearch {
            hosts => [“http://localhost:9200"]
            index => “users”
            document_id => “users_%{userid}”
            doc_as_upsert => true
            #user => “es_user”
            #password => “es_password”
        }
    }

<hr>

# AWS OpenSearch tutorial 

**base reference**: https://github.com/johnny-chivers/amazon-opensearch-service



## putting data

**single document**:

curl -XPUT -u 'openuser:DevP2024#' 'https://search-testesearch-uhyimewofpyjtlbjsixeuy5hru.us-east-1.es.amazonaws.com/movies/_doc/2' -d '{"director": "Burton, Tim", "genre": ["Comedy","Sci-Fi"], "year": 1996, "actor": ["Jack Nicholson","Pierce Brosnan","Sarah Jessica Parker"], "title": "Mars Attacks! 233"}' -H 'Content-Type: application/json'

curl -XPUT -u 'openuser:DevP2024#' 'http://localhost:5601/movies/_doc/1' -d '{"director": "Burton, Tim", "genre": ["Comedy","Sci-Fi"], "year": 1996, "actor": ["Jack Nicholson","Pierce Brosnan","Sarah Jessica Parker"], "title": "Mars Attacks!"}' -H 'Content-Type: application/json'

**multiple documents**

curl -XPOST -u 'master-user:master-user-password' 'domain-endpoint/_bulk' --data-binary @bulk_movies.json -H 'Content-Type: application/json'


## Searching data 

**search document**

curl -XGET -u 'openuser:DevP2024#' 'https://search-testesearch-uhyimewofpyjtlbjsixeuy5hru.us-east-1.es.amazonaws.com/movies/_search?q=pedro&pretty=true'




curl -XPUT -u 'openuser:DevP2024#' 'https://search-testesearch-uhyimewofpyjtlbjsixeuy5hru.us-east-1.es.amazonaws.com/movies/_doc/3' -d '{"director": "Burton, Tim", "genre": ["Comedy","Sci-Fi"], "year": 1996, "actor": ["Pierce Brosnan","Sarah Jessica Parker"], "title": "Mars Attacks! 5"}' -H 'Content-Type: application/json'



curl -XPUT -u 'openuser:DevP2024#' 'https://search-testesearch-uhyimewofpyjtlbjsixeuy5hru.us-east-1.es.amazonaws.com/movies/_doc/' -d '{"director": "Burton, Tim", "genre": ["Comedy","Sci-Fi"], "year": 1996, "actor": ["Pedro3 will","Pierce Brosnan","Sarah Jessica Parker"], "title": "Mars Attacks! 7"}' -H 'Content-Type: application/json'