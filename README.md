Steps to populate required data:


Index creation,

curl -X PUT "localhost:9200/test?pretty" -H 'Content-Type: application/json' -d'{"settings": {"number_of_shards": 1,"number_of_replicas": 0},"mappings": {"properties": {"messagetime": { "type": "date" }}}}'

Index bulk docs,

curl -s -H "Content-Type: application/x-ndjson" -XPOST localhost:9200/test/_bulk --data-binary "@bulkfile"; echo

Search query,

curl -X POST "localhost:9200/test/_search?size=0&pretty" -H 'Content-Type: application/json' -d'
    {
	  "explain": false,
	  "size": 0,
	  "query": {
	    "bool": {
	      "adjust_pure_negative": true,
	      "must": [
		{
		  "range": {
		    "messagetime": {
		      "include_lower": true,
		      "include_upper": true,
		      "from": 1710061200000,
		      "to": 1710068400000
		    }
		  }
		}
	      ]
	    }
	  },
	  "from": 0,
	  "aggregations": {
	    "timeslice|timehisto": {
	      "date_histogram": {
		"field": "messagetime",
		"offset": 660000,
		"fixed_interval": "43m",
		"time_zone": "America/Los_Angeles",
		"keyed": false,
		"min_doc_count": 0,
		"order": {
		  "_key": "asc"
		},
		"extended_bounds": {
		  "min": 1710061200000,
		  "max": 1710068400000
		}
	      }
	    }
	  },
	  "track_total_hits": true
    }'
