# CQL3
--


### Table with single row-key value

```
create table Demo {
  rkey1 text,
  col1 text,
  col2 text,
  PRIMARY KEY (rkey1)
};
insert rkey1, col1, col2 values AAA, A1, A2;
insert rkey1, col1, col2 values BBB, B1, B2;
```

 rkey1 | col1 | col2 |
-------|------|------|
  AAA  | A1   | A2   |
  BBB  | B1   | B2   |


### Table with single row-key and column value prefix

```
create table Demo {
  rkey1 text,
  ckey1 text,
  col2 text,
  PRIMARY KEY (rkey1,ckey1)
};
insert rkey1, ckey1, col1 values AAA, A1, A2;
insert rkey1, ckey1, col1 values BBB, B1, B2;
```


 rkey1 | A1:c2 | B1:c2 |
-------|-------|-------|
  AAA  |    A2 |       |
  BBB  |       |    B2 |


### Table with composite row-key and composite column value prefix

```
create table Demo {
  rkey1 text,
  rkey2 text,
  ckey1 text,
  ckey2 text,
  col3 text,
  PRIMARY KEY ((rkey1,rkey2),ckey1,ckey2)
};
insert rkey1, rkey2, ckey1, ckey2, col3 values aaa, AAA, A1, A2, A3
```

 rkey1:rkey2 | A1:A2:c3 | B1:B2:c3 |
-------------|----------|----------|
  aaa:AAA    |       A3 |          |
  bbb:BBB    |          |       B3 |


### Table with `set` collection column values

```
create table Demo {
  rkey1 text,
  col1 set,
  col2 text,
  PRIMARY KEY (rkey1)
};
insert rkey1, col1, col2 values AAA, { S1, S2, S3 }, A2;
insert rkey1, col1, col2 values BBB, { S1, S2, S3 }, A2;
```

 rkey1 | col1:S1 | col1:S2 | col1:S3 | col2 |
-------|---------|---------|---------|------|
  AAA  |         |         |         | A2   |
  BBB  |         |         |         | B2   |


### Table with `map` collection column values

```
create table Demo {
  rkey1 text,
  col1 map,
  col2 text,
  PRIMARY KEY (rkey1) 
};
insert rkey1, col1, col2 values AAA, { S1:V1, S2:V2, S3:V3 }, A2;
insert rkey1, col1, col2 values BBB, { S1:V1, S2:V2, S3:V3 }, A2;
```

 rkey1 | col1:S1 | col1:S2 | col1:S3 | col2 |
-------|---------|---------|---------|------|
  AAA  |      V1 |      V2 |      V3 | A2   |
  BBB  |      V1 |      V2 |      V3 | B2   |


### Table with `list` collection column values

```
create table Demo {
  rkey1 text,
  col1 list,
  col2 text,
  PRIMARY KEY (rkey1)
};
insert rkey1, col1, col2 values AAA, [ S1, S2, S3 ], A2;
insert rkey1, col1, col2 values BBB, [ S1, S2, S3 ], A2;
```

 rkey1 | col1:01 | col1:02 | col1:03 | col2 |
 ------|---------|---------|---------|------|
  AAA  |      S1 |      S2 |      S3 | A2   |
  BBB  |      S1 |      S2 |      S3 | B2   |


## CASSANDRA

#### Single Node
All on one server, all read/writes to same process.

#### Sharding
Different pieces of data held by different node instances, thus data groups are isolated from one another.

#### Master/Slave Replication
Data is copied to other slave nodes, with master node the diffinitive golden source usually responsible for updates.
Peer to Peer Replication:  All nodes equals, all read/write, share their writes with other nodes.

#### Sharding & Replication a common strategy
Master/Slave with Sharding or Peer-to-Peer with Sharding (replication factor of 3 i.e. a dataset is replicated to 3 nodes so the loss of single node can be recovered from one of the other two) 

#### Create & Insert

`./cqlsh -3 localhost 9160`

```
CREATE KEYSPACE twissandra
  with strategy_class = 'SimpleStrategy'
  and strategy_options:replication_factor=1;

use twissandra;

CREATE TABLE timeline(
    user_id       varchar,
    tweet_id      varchar,
    tweet_device  varchar,
    author        varchar,
    body          varchar,
    PRIMARY KEY(user_id,tweet_id,tweet_device));

INSERT INTO timeline (user_id, tweet_id, tweet_device, author, body)
VALUES ('xamry', 't1', 'web', 'Amresh', 'Here is my first tweet');

INSERT INTO timeline (user_id, tweet_id, tweet_device, author, body)
VALUES ('xamry', 't2', 'sms', 'Saurabh', 'Howz life Xamry');

INSERT INTO timeline (user_id, tweet_id, tweet_device, author, body)
VALUES ('mevivs', 't1', 'iPad', 'Kuldeep', 'You der?');

INSERT INTO timeline (user_id, tweet_id, tweet_device, author, body)
VALUES ('mevivs', 't2', 'mobile', 'Vivek', 'Yep, I suppose');
```

#### Select

`cqlsh:twissandra> select * from timeline;`

 user_id | tweet_id | author  | body
---------|----------|---------|------------------------
   xamry |       t1 |  Amresh | Here is my first tweet
   xamry |       t2 | Saurabh |        Howz life Xamry
  mevivs |       t1 | Kuldeep |               You der?
  mevivs |       t2 |   Vivek |         Yep, I suppose


`cqlsh:twissandra> SELECT * FROM timeline WHERE user_id='xamry';`

 user_id | tweet_id | tweet_device | author  | body
---------|----------|--------------|---------|------------------------
   xamry |       t1 |          web |  Amresh | Here is my first tweet
   xamry |       t2 |          sms | Saurabh |        Howz life Xamry


`cqlsh:twissandra> select * from timeline where tweet_id = 't1';`

 user_id | tweet_id | tweet_device | author  | body
---------|----------|--------------|---------|------------------------
   xamry |       t1 |          web |  Amresh | Here is my first tweet
  mevivs |       t1 |         iPad | Kuldeep |               You der?


`cqlsh:twissandra> select * from timeline where user_id = 'xamry' and tweet_id='t1';`

 user_id | tweet_id | tweet_device | author | body
---------|----------|--------------|--------|------------------------
   xamry |       t1 |          web | Amresh | Here is my first tweet


`cqlsh:twissandra> select * from timeline where user_id = 'xamry' and author='Amresh';`

Bad Request: No indexed columns present in by-columns clause with Equal operator

`cqlsh:twissandra> select * from timeline where user_id = 'xamry' and tweet_device='web';`

Bad Request: PRIMARY KEY part tweet_device cannot be restricted (preceding part tweet_id is either not restricted or by a non-EQ relation)

`cqlsh:twissandra> select * from timeline where user_id = 'xamry' and tweet_id = 't1' and tweet_device='web';`

 user_id | tweet_id | tweet_device | author | body
---------|----------|--------------|--------|------------------------
   xamry |       t1 |          web | Amresh | Here is my first tweet


```
$ ./cassandra-cli -h localhost -p 9160
Connected to: "Test Cluster" on localhost/9160
Welcome to Cassandra CLI version 1.1.2

Type 'help;' or '?' for help.
Type 'quit;' or 'exit;' to quit.

[default@unknown] use twissandra;
Authenticated to keyspace: twissandra
[default@twissandra] list timeline;
<pre>Using default limit of 100
Using default column limit of 100
-------------------
RowKey: xamry
=> (column=t1:web:author, value=Amresh, timestamp=1343729388951000)
=> (column=t1:web:body, value=Here is my first tweet, timestamp=1343729388951001)
=> (column=t2:sms:author, value=Saurabh, timestamp=1343729388973000)
=> (column=t2:sms:body, value=Howz life Xamry, timestamp=1343729388973001)
-------------------
RowKey: mevivs
=> (column=t1:iPad:author, value=Kuldeep, timestamp=1343729388991000)
=> (column=t1:iPad:body, value=You der?, timestamp=1343729388991001)
=> (column=t2:mobile:author, value=Vivek, timestamp=1343729389941000)
=> (column=t2:mobile:body, value=Yep, I suppose, timestamp=1343729389941001)
```

