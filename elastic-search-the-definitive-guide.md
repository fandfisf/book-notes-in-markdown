# ElasitcSearch
All credit -  <https://www.elastic.co/guide/en/elasticsearch/guide/current/getting-started.html>
## Getting Started
* ElasticSearch (_ES_) is "Document Oriented". Rather than view the data-world as a collection of giant spread-sheets, ES views it as a collection of _documents_.
* ES uses __JSON__ as the serialization format for the documents it stores.
---
###Some Definitions
##### Index
A place to store documents, much like an RDBMS
#####Indexing
The act of storing document in a database
#####_Inverted_ Index
Have **no idea** what this is yet!
###Remember this
* By default every field in a document is indexed

###Adding data
`[cluster [index [document <is of> type] ] ]`
* When we add data, we _index_ a _document_ per instance (employee, car, student etc.)
* Each document will be of a certain _type_ (Again,
 employee, car, student etc.)
* The document lives inside an _index_
* The index lives inside a _cluster_
* Standard HTTP verbs are used (`PUT` in this case)
###Finding data
* Simple query that gets everything `GET /<index-name>/<type-name>/_search`
* A "field-based query" `GET /<index-name>/<type-name>/_search?q=<field-name>:<value-to-look-for>`
* A DSL based query with field matching
```
GET /school/student/_search
{
  "query":{
    "match":{
      "last_name":"learner"
    }
  }
}
```
* A DSL based query that uses filter in addition to a query criteria
```
GET /school/student/_search
{
  "query" : {
      "bool": {
          "must": [
              "match" : {
                  "last_name" : "smith"
              }
          ],
          "filter": {
              "range" : {
                  "age" : { "lt" : 10 }
              }
          }
      }
  }
}
```
* Full text search
```
# Find all employees who enjoy "rock" or "climbing"
GET /megacorp/employee/_search
{
    "query" : {
        "match" : {
            "about" : "rock climbing"
        }
    }
}
```
```
# Find all employees who enjoy "rock climbing"
GET /megacorp/employee/_search
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    }
}
```
```
# Find all employees who enjoy "rock climbing"
# and highlight the matches
GET /megacorp/employee/_search
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    },
    "highlight": {
        "fields" : {
            "about" : {}
        }
    }
}
```
#### Relevance
The last query above, returns results with **relevance score**. This is very important for full-text searches. Also the results have the search words enclosed in <em> tag.
```
"hits": {
  "total": 2,
  "max_score": 0.16273327,
  "hits": [
    {
      "_index": "megacorp",
      "_type": "employee",
      "_id": "1",
      "_score": 0.16273327,
      "_source": {
        "first_name": "John",
...
```
