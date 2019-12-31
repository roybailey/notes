----
## SPARK

* written in Scala
* supports stream processing, graph, SQL access, batch processing
* supports interactive shell for data exploration
* suits existing cluster multi-processing requirements


----
## STORM

* written in Closure
* supports stream processing
* many spouts (integration adaptors like EMS, Kafka)
* multi-language support through json API
* suits greenfield purpose built cluster specifically for stream processing


----
## LDAP

## LDCP?

----
## CAP `Consistency` `Availability` `Partition tolerance`
`Consistency` - all nodes see the same data at the same time

`Availability` - a guarantee that every request receives a response about whether it was successful or failed

`Partition tolerance` - the system continues to operate despite arbitrary message loss or failure of part of the system

In theoretical computer science, the CAP theorem, also known as Brewer's theorem, states that it is impossible for a distributed computer system to simultaneously provide all three of the following guarantees:[1][2][3]

## ACID `Atomic` `Consistent` `Isolated` `Durable`
_guarantee that database transactions are processed reliably_

`Atomicity` - requires that each transaction is "all or nothing": if one part of the transaction fails, the entire transaction fails, and the database state is left unchanged.

`Consistency` database always in valid state (all rules, constraints, triggers etc. are completed within update)

`Isolation` - ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed serially, i.e. one after the other.

`Durability` - means that once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors.


## BASE `Basically` `Available` `Soft-state` `Eventually consistent`

the BASE methodology is characterized by high availability for first-tier services, leaving some kind of background cleanup mechanism to resolve any problems created by optimistic actions that later turn out to have violated consistency

----
## Types of Databases

#### Relational
Traditional databases usually use SQL to model and query data. They are useful for data which can be stored in a highly structured schema, yet require flexible querying. Scaling a relational database (RDBMS) traditionally occurs by more powerful hardware (vertical growth).

Examples: PostgreSQL, MySQL, Oracle

#### Graph
These exist for highly interconnected data. They excel in modeling complex relationships between nodes, and many implementations can handle multiple billions of nodes and relationships (or edges and vertices). I tend to include triplestores and object DBs to be specialized variants.

Examples: Neo4j, Graphbase, InfiniteGraph, OrientDB

#### Document
Document datastores model hierarchical values called documents, represented in formats such as JSON or XML, and do not enforce a document schema. They generally support distributing across multiple servers (horizontal growth).

Examples: CouchDB, MongoDB, Couchbase

#### Columnar
Popularized by Google's BigTable, this form of database exists to scale across multiple servers, and groups similar data into column families. Column values can be individually versioned and managed, though families are defined in advance, not unlike RDBMS schemas.

Examples: HBase, Cassandra, BigTable

#### Key/Value
Key/Value, or KV stores, are conceptually like hashtables, where values are stored and accessed by an immutable key. They range from single-server varieties like Memcached used for high-speed caching, to multi-datacenter distributed systems like Riak Enterprise.

Examples: Riak, Redis, Voldemort
