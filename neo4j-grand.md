
`http://bit.ly/grandstackslides`


Apollo, client and server side
Schema Stitching (binding multiple GraphQL schema together)

`http://neo4jsandbox.com` (GitHub login)

```
match (m:Movie)<-[r:RATED]-(u:User)
where m.title contains "River Runs Through It"
return * limit 10
```


`:schema` return schema DDL

`call db.schema()` returns schema graph

Match "River Runs Through It" movie title that any user rated
and other movies those people also rated showing most popular

```
match (m:Movie)<-[r:RATED]-(u:User)-[:RATED]->(other:Movie)
where m.title contains "River Runs Through It"
return other.title as other_movie, count(*) as num
order by num desc limit 10
```


Match movies that any user rated and other movies those people
also rated showing most popular

```
match (m:Movie)<-[r:RATED]-(u:User)-[:RATED]->(rec:Movie)
where m.title = 'Matrix, The'
return rec.title as recommendation, count(*) as usersWhoAlsoWatched
order by usersWhoAlsoWatched desc limit 10
```

Recommended movie for given movie

```
MATCH (m:Movie) WHERE m.movieId = $movieId
MATCH (m)-[:IN_GENRE]->(g:Genre)<-[:IN_GENRE]-(movie:Movie)
WITH m, movie, COUNT(*) AS genreOverlap
MATCH (m)<-[:RATED]-(:User)-[:RATED]->(movie:Movie)
WITH movie,genreOverlap, COUNT(*) AS userRatedScore
RETURN movie ORDER BY (0.9 * genreOverlap) + (0.1 * userRatedScore)  DESC LIMIT 3
```

Setup Neo4j recommendations database

```
curl https://s3.amazonaws.com/neo4j-sandbox-usecase-datastores/v3_2/recommendations.db.zip > /tmp/recommandations.zip
cd /tmp
unzip recommendations.zip
mkdir -p neo4j/data/databases/
mv recommendations.db neo4j/data/databases/graph.db
docker run \
  --publish=7474:7474 --publish=7687:7687 \
  --volume=/tmp/neo4j/data:/data \
  neo4j:3.2.3
``` 


`:param title = "The Matrix"` to assign browser parameter
`match (m:Movie { title: $title }) return m`

`match (m:Movie) where m.title contains $title return m { .title, .released }` shorthand multi-property return 
