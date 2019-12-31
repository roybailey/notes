# Neo4J
--

- ACID
- Clustered: Master+Slaves (Master on writes)
- Replication Factor settings
- Http+JSon api
- Java plugins (like stored procs) - can act like triggers

#### Nodes, Relationships, Properties, Labels

**Nodes** contains properties (key+value or value[])

**Nodes** = Identity (e.g. Author, Book, Person)

**Nodes** = Complex Value object (e.g. Address, multiple properties making unit)

**Relationships** = Connect nodes with Name+Direction

**Relationships** contains properties (key+value or value[])

**Relationships** properties tend to denote quality (weight of relationship) e.g. strength, date+time, distance etc.

**Relationships** must have startNode+endNode (no dangling relationships)

**Relationships** must be fully deleted before deleting Nodes

**Nodes** can have >1 relationship same name

**Nodes** can have cyclic relationships

**Nodes** can have self relationships

**Relationships** defined at 'instance' level, not 'class/table' level (diff from foreign keys)

**Relationships** stored on disk with data, hence no multi-join required at query time, traversing fast

**Label** defines Node types,roles,groups e.g. user, product, company

**Indexes** and constraints against Labeled nodes (e.g. Nodes labeled Person have unique name)

**Hierarchy Taxonomy** defined in own graph, relationships from other instance graphs to that taxonomy
No implicit metadata (e.g. created etc must be manually added)


#### Cypher Query

`()-->()` = Node to Node via Relationship
e.g. `(:Person)-[:FRIEND]->(:Person)`

**Properties** use json like syntax `(:Person{name:'Peter'})-[:FRIEND{key:'value'}]->(:Person{name:'Lucy'})`

**Identifiers** allows variables to be defined/used later in query like an assigned identifier (inc. bound to collections)
e.g. `(p1:Person{name:'Peter'})-[r:FRIEND{key:'value'}]->(p2:Person{name:'Lucy'})`

**Cypher Structure**
```
MATCH graph_pattern
WHERE binding_and_filter_criteria
RETURN results
```

friends here is the collection of nodes given a variable name (hence no colon)
```
MATCH (p:Person)-[:FRIEND]->(friends)
WHERE p.name = 'Peter'
RETURN friends
```
```
MATCH (company)<-[:WORKS_FOR]-(me:Person)-[:HAS_SKILL]->(skill),
      (company)<-[:WORKS_FOR]-(colleague)-[r:HAS_SKILL]->(skill)    <--- colleague company and skill must match
WHERE  me.username = 'ian'
       AND r.level > 1
RETURN colleague.username AS username,
       count(skill) AS score,            <-- counts common skills with me
       collect(skill.name) AS skills     <-- creates comma separated list of skills
ORDER BY score DESC
```

#### Symmetric Relationship (e.g. PARENT_OF and CHILD_OF)

preference to use one half relationship, use query to infer other direction
```
MATCH (parent)-[:PARENT_OF]->(child)
WHERE parent.name = 'sarah'
RETURN child
same as (through binding of parent or child name)
MATCH (parent)-[:PARENT_OF]->(child)
WHERE child.name = 'eric'
RETURN child
```

#### Bi-Directional Relationship

dont exist
modeled as two relationships, one each direction (e.g. when you need different quality rating)
or model as one and remove the direction from query

#### Properties versus Relationship

- use relationship when quality/weight/strength is of value
- use relationship when value is complex (e.g. address)
- use property when there is no need quality
- use property when simple value
- faster to store as property than relationship, but relationships faster that sql joins
- lookups on large strings or large arrays can impact performance
- well named relationships to eliminate unnecessary portions of graph
- multiple relationships of same information but targeting different queries/use-cases valid

#### Creating Data
merge to create or fetch unique node
```
MERGE (c:Company{name:'Acme'})
MERGE (p:Person{username:'bill'})
MERGE (s1:Skill{name:'Neo4j'})
MERGE (s2:Skill{name:'Ruby'})
CREATE UNIQUE (c)<-[:WORKS_FOR]-(p),
    (p)-[r1:HAS_SKILL]->(s1),
    (p)-[r2:HAS_SKILL]->(s2)
SET r1.level = 1
SET r2.level = 2
RETURN c, p, s1, s2
```

linked lists with relationships (next)
interleaved linked lists (various relationships to represent diff orders over the list)

pointers to head and tail e.g. node season 12 with first/last relationship to episodes, episodes have next links

timeline trees e.g. `timeline:name -> year:# -> Month:# -> Day:#`

Timestamps:
```
MERGE ...
ON MATCH set node.updated = timestamp()
ON CREATE set node.created = timestamp()
```

- 10-15 nodes per Cypher query (as guideline)

- 20-50k nodes/relationships per/transaction (as guideline)
neo-shell$ profile match(...) where ... <--- shows explain plan, read bottom up
java-api 'tx-size' commit count

- gist.neo4j.org (GraphGist) examples of graphs (e.g. versioning)

- wide v shallow node trees traverse from bottom up for speed


e.g. Which People that work for same Company as me have similar Skills
```
MATCH (company)<-[:WORKS_FOR]-(me:Person)->[:HAS_SKILL]->(skill),
      (company)<-[:WORKS_FOR]-(colleague)->[:HAS_SKILL]->(skill)
WHERE me.name = {name}
RETURN colleague.name as name,
       count(skill) AS score,
       collect(skill.name) AS skills
ORDER BY score DESC
```

## Import Examples
```
MERGE (ass:Asset{ key:'EQUITY' })
MERGE (src:Source{ key:'RDDH' })
MERGE (ubs:Synonym{ key:'UBSID' })
MERGE (irf:InstrumentReference{ key:'EQUITY/RDDH/UBSID/U0001' })
CREATE (irf)-[:CLASSIFIED_AS]->(ass)
CREATE (irf)-[:SOURCED_FROM]->(src)
CREATE (irf)-[:IDENTIFIED_AS{identifier:'U0001'}]->(ubs);
```

```
BEGIN

CREATE (mum:Person{name:'Elaine',born:1976});
CREATE (dad:Person{name:'Rob',born:1975});
CREATE (lily:Person{name:'Lily',born:2006});
CREATE (george:Person{name:'George',born:2008});

MATCH (mum:Person), (george:Person)
WHERE mum.name='Elaine' AND george.name='George'
CREATE (mum)-[:PARENT_OF]->(george);
MATCH (dad:Person), (george:Person)
WHERE dad.name='Rob' AND george.name='George'
CREATE (dad)-[:PARENT_OF]->(george);

MATCH (mum:Person), (lily:Person)
WHERE mum.name='Elaine' AND lily.name='Lily'
CREATE (mum)-[:PARENT_OF]->(lily);
MATCH (dad:Person), (lily:Person)
WHERE dad.name='Rob' AND lily.name='Lily'
CREATE (dad)-[:PARENT_OF]->(lily);

MATCH (mum:Person), (dad:Person)
WHERE mum.name='Elaine' AND dad.name='Rob'
CREATE (mum)<-[:MARRIED_TO]-(dad);
MATCH (mum:Person), (dad:Person)
WHERE mum.name='Elaine' AND dad.name='Rob'
CREATE (mum)-[:MARRIED_TO]->(dad);

CREATE (gp1:Person{name:'Jim',born:1946});
CREATE (gp1:Person{name:'Fred',born:1945});
CREATE (gp1:Person{name:'Jean',born:1948});
CREATE (gp1:Person{name:'Rose',born:1945});

MATCH (gp:Person), (p:Person)
WHERE gp.name='Fred' AND p.name='Rob'
CREATE (gp)-[:PARENT_OF]->(p);
MATCH (gp:Person), (p:Person)
WHERE gp.name='Rose' AND p.name='Rob'
CREATE (gp)-[:PARENT_OF]->(p);

MATCH (gp:Person), (p:Person)
WHERE gp.name='Jim' AND p.name='Elaine'
CREATE (gp)-[:PARENT_OF]->(p);
MATCH (gp:Person), (p:Person)
WHERE gp.name='Jean' AND p.name='Elaine'
CREATE (gp)-[:PARENT_OF]->(p);

COMMIT
```


### Loading Data

Given a csv file such as this:

```
Category,Name,Strategy,Score
Platform,C++,Reduce,26.5
Platform,Flex,Reduce,26.5
Platform,iOS,Keep,26.5
Platform,Java,Keep,26.5
Platform,Web,Increase,26.5
Technical Component,29West,Reduce,26.5
Technical Component,App Dynamics,Increase,26.5
```

We can load into Neo4j with this:

```
LOAD CSV WITH HEADERS FROM "file:/Users/roybailey/Coding/gitpoc/technical-estate.csv" AS row RETURN row
```

```
// load with properties
LOAD CSV WITH HEADERS FROM "file:/Users/roybailey/Coding/gitpoc/technical-estate.csv" AS row
WITH row
WHERE row.Name IS NOT NULL
MERGE (n:Skill {type: row.Category, name: row.Name, score:row.Score})
RETURN n
```

```
Name,Skill,Score
Platform,C++,Reduce,26.5
Platform,Flex,Reduce,26.5
Platform,iOS,Keep,26.5
Platform,Java,Keep,26.5
Platform,Web,Increase,26.5
Technical Component,29West,Reduce,26.5
Technical Component,App Dynamics,Increase,26.5
```

```
// load with properties
LOAD CSV WITH HEADERS FROM "file:/Users/roybailey/Coding/gitpoc/resource-pool.csv" AS row
WITH row
WHERE row.Name IS NOT NULL
MERGE (n:Resource {name: row.Name, score:row.Score})
MERGE (s:Skill {name: row.Skill})
MERGE (n)-[:HAS_SKILL]->(s)
RETURN n
```

# MEGA LOAD Users and Friends Graph
```

UNWIND range(1, 1000000) AS x
CREATE (:Person {id: x, name: 'name' + x, age: x % 100}))

UNWIND range(1, 1000000) AS x
MATCH (n)
WHERE id(n) = x
MATCH (m)
WHERE id(m) = toInt(rand() * 1000000)
CREATE (n)-[:KNOWS]->(m)
```
