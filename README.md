Steps to populate required data:


Index creation,

curl -X PUT "localhost:9200/test?pretty" -H 'Content-Type: application/json' -d'{"settings": {"number_of_shards": 1,"number_of_replicas": 0},"mappings": {"properties": {"messagetime": { "type": "date" }}}}'

Index bulk docs,

curl -s -H "Content-Type: application/x-ndjson" -XPOST localhost:9200/test/_bulk --data-binary "@bulkfile"; echo


