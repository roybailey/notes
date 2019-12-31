# DataStax
--

* jupyter notebook
* `solrconfig.xml`
* `schema.xml`

## SOLR

* `Document` = row
* `Term` = search word
* `Phrase` = combined words by quote


### SOLR search

* Expression = `field:term`
* All documents = `*:*`
* Basic CQL construct `select * from <table> where solr_query = 'field:term'`
* e.g. `select * from films where solr_query = 'title:tombstone'`
* Boolean operators (`AND`, `OR`, `PROHIBIT`)
* Grouping multiple terms together (default operator OR)
```
title:(full metal jacket)
title:(full OR metal OR jacket)
title:(full AND (metal OR jacket))
```
* Multiple fields
```
genres:action title:(full OR metal OR jacket)
```
* MultiValue field = lists/maps/sets
* `<copyField source="" dest="" />` allows you to create different types of same field value index
* `dsetool create_core keyspace.table generateResources=true reindex=true`
* `dsetool get_core_schema keyspace.table`
* changes to solr config needs reloading into core solr engine and data reindexing
* `dsetool reload_core keyspace.table generateResources=true reindex=true`
* Facets breaks results into categories
* Facet buckets can be field values, query result, or field range calculated buckets (start, end, gap e.g. 0-100 in 10s)
* `select * from <table> where solr_query = '{"g":"generes_facet","facet":{field|query|range}}'`
* range facet start=inclusive end=exclusive


## SPARK

* `Document` = row
* `Term` = search word
* `Phrase` = combined words by quote
