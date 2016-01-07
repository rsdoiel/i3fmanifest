
These are notes on a hypothetical IxIF manifest service.

# Notes on Islandora/Solr/Fedora APIs

## client libraries Solr

+ [gosolr](https://github.com/droxer/gosolr) appears to be active, most others are a few years old
+ [Go-Solr](https://github.com/rtt/Go-Solr/) maybe stale, 2 years old, last pull request May 11

## Fedora Repositories

### v3

Used by current Fedora.

+ [REST+API](https://wiki.duraspace.org/display/FEDORA38/REST+API)

### v4

Used by current ArchivesSpace.

+ [RESTful+HTTP+API](https://wiki.duraspace.org/display/FEDORA4x/RESTful+HTTP+API)

## URLs

+ [Islandora/Solr](http://localhost:8080/solr/#/) for vagrant developer image


Example URL for querying my dev islandora_vagrant deployment for the PDF name 'Essentials Bash v1'

```
    http://localhost:8080/solr/collection1/select?q=dc.title%3A*Bash*%0A&fl=PID%2Cdc.title&wt=json&indent=true
```

The results look like

```json
    {
       "responseHeader":{
       "status":0,
       "QTime":2,
       "params":{
          "q":"dc.title:*Bash*\n",
          "indent":"true",
          "fl":"PID,dc.title",
          "wt":"json"}},
       "response":{"numFound":1,"start":0,"docs":[
          {
            "PID":"islandora:5",
            "dc.title":["Essentials Bash v1"]}]
          }}
```

+ _q_ is used to setup a query which takes a fieldname, colon and the value to search for
+ _fl_ takes a comma delimited list of fields to return in results
+ _wt_ sets the format (e.g. JSON, XML)
+ _indent_ takes a 'true' or 'false' and turns on/off indentation in the output
    + false is what you want in production to minimize response size

Noticed that our islandora Solr returns JSON responses with a mime-type of text/plain. This may cause problems in client interactions. XML
responses seem to have the correct mime-type. Is this a version issue or a general issue of configuring jetty?


## Fields of interest in Islandora

+ dc.title
+ db.subject
+ dc.description
+ dc.contributor
+ dc.type
+ dc.identifier
+ dc.source
+ dc.date
+ cd.creator
+ FULL_TEXT_t

Example of setting these fields for output.

```
    fl=PID,dc.*
```

This seems to get the helpful dublin core fields for all the records (second one included PID as well as dc.* fields).

+ [All OBJ records, all dc.* fields](http://localhost:8080/solr/collection1/select?q=fedora_datastream_info_OBJ_ID_mt%3AOBJ&fl=dc.*&wt=json&indent=true)
+ [All OBJ records, dc.* and PID](http://localhost:8080/solr/collection1/select?q=fedora_datastream_info_OBJ_ID_mt%3AOBJ&fl=PID%2Cdc.*&wt=json&indent=true)

Getting just the PID for records that are of defora_datastream_info_OBJ_ID_mt

+ [All OBJ records returning only PID field](http://localhost:8080/solr/collection1/select?q=fedora_datastream_info_OBJ_ID_mt%3AOBJ&fl=PID&wt=json&indent=true)

Getting a specific record

+ [Getting single record islandora:5 with PID and dc.* fields](http://localhost:8080/solr/collection1/select?q=PID%3Aislandora%5C%3A8&fl=PID%2Cdc.*&wt=json&indent=true)

This MODS fields also maybe helpful but they have allot of redudent data.

Get all my PIDs

```
    http://localhost:8080/solr/collection1/select?q=fedora_datastream_info_OBJ_ID_mt%3AOBJ&fl=PID&wt=json&indent=true
```

Returns something like

```
    {
        "responseHeader":{
            "status":0,
            "QTime":0,
            "params":{
                "q":"fedora_datastream_info_OBJ_ID_mt:OBJ",
                "indent":"true",
                "fl":"PID",
                "wt":"json"
            }
        },
        "response":{
            "numFound":6,
            "start":0,
            "docs": [
                {
                    "PID":"islandora:5"
                },
                {
                    "PID":"islandora:6"
                },
                {
                    "PID":"islandora:7"
                },
                {
                    "PID":"islandora:8"
                },
                {
                    "PID":"islandora:9"
                },
                {
                    "PID":"islandora:10"
                }
            ]
        }
    }
```

Use rows=ROWS_PER_REQUEST_COUNT and start=COUNT_INTO_RESULTS to manage paging.

```
    http://localhost:8080/solr/collection1/select?q=fedora_datastream_info_OBJ_ID_mt%3AOBJ&fl=PID&wt=json&indent=true&rows=3&start=3
```

Count starts from zero to n-1 (e.g. rows=3&start=0 would be the first three results and the next would be rows=3&start=3 for next set.).

Example returning PID, title and description paged three at a time would look like

```
    http://localhost:8080/solr/collection1/select?q=fedora_datastream_info_OBJ_ID_mt%3AOBJ&fl=PID,dc.title,dc.description&wt=json&indent=true&rows=3&start=0
```

With results like

```    
    {
        "responseHeader": {
            "status":0,
            "QTime":0,
            "params": {
                "q":"fedora_datastream_info_OBJ_ID_mt:OBJ",
                "indent":"true",
                "fl":"PID,dc.title,dc.description",
                "start":"0",
                "rows":"3",
                "wt":"json"
            }
        },
        "response": {
            "numFound":6,
            "start":0,
            "docs":[
                {
                    "PID":"islandora:5",
                    "dc.title":["Essentials Bash v1"],
                    "dc.description":["Introduction to Bash and Unix command line."]
                },
                {
                    "PID":"islandora:6",
                    "dc.title":["The Mag Pi Issue 35"],
                    "dc.description":["Articles for July 2015 issue of the Mag Pi magazine."]
                },
                {
                    "PID":"islandora:7",
                    "dc.title":["The Mag Pi Issue 36"],
                    "dc.description":["This August 2015 edition of Mag Pi Magazine."]
                }
            ]
        }
    }
```
