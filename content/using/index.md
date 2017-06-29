---
menu: main
title: Using HodDB
weight: 3
---

- [Loading Data](#loadingdata)
	- [Loading Brick Files](#loadingbrick)
- [CLI Queries](#cliqueries)
	- [CLI Configuration Options](#cliconfigs)
- [HTTP API](#httpapi)
- [HTTP Frontend](#httpfront)
- [BOSSWAVE API](#bosswaveapi)
- [Tools](#tools)

<a name="loadingdata"></a>
### Loading Data

#### Overview

In Brick, buildings are described using RDF triples. These triples are stored in [Turtle (`.ttl`) files](https://www.w3.org/TR/turtle/), which can be loaded into a HodDB instance.

HodDB stores buildings in a set of LevelDB databases; the location of these files is determined by the `DBPath` configuration option which defaults to `./_hoddb`.

In order to perform queries on the the database using the Brick classes and relationships, HodDB also needs to import the Turtle files that define the Brick schema. The location of these files are determined by the `BrickFrameTTL` and `BrickClassTTL` configuration options. The most recent version of these files can be downloaded from [http://brickschema.org/download/](http://brickschema.org/download/).

<a name="loadingbrick"></a>
#### Loading Brick Files

In order to load a `.ttl` file, we use the `hod load` command:

```bash
$ hod load buildings/berkeley.ttl
INFO actions.go:50 Dec 24 13:49:30  ▶ Loaded 3774 triples, 8 namespaces in 128.412302ms (29390/sec)
INFO db.go:454 Dec 24 13:49:31  ▶ Built lookup tables in 163.014409ms
INFO db.go:460 Dec 24 13:49:31  ▶ Built graph in 421.345208ms
rdf => http://www.w3.org/1999/02/22-rdf-syntax-ns
rdfs => http://www.w3.org/2000/01/rdf-schema
skos => http://www.w3.org/2004/02/skos/core
bf => http://buildsys.org/ontologies/BrickFrame
tag => http://buildsys.org/ontologies/BrickTag
 => http://buildsys.org/ontologies/Brick
owl => http://www.w3.org/2002/07/owl
INFO db.go:454 Dec 24 13:49:32  ▶ Built lookup tables in 185.460746ms
INFO db.go:460 Dec 24 13:49:32  ▶ Built graph in 143.048733ms
tag => http://buildsys.org/ontologies/BrickTag
 => http://buildsys.org/ontologies/Brick
btag => http://buildsys.org/ontologies/BrickTag
skos => http://www.w3.org/2004/02/skos/core
bf => http://buildsys.org/ontologies/BrickFrame
soda_hall => http://buildsys.org/ontologies/building_example
xml => http://www.w3.org/XML/1998/namespace
xsd => http://www.w3.org/2001/XMLSchema
owl => http://www.w3.org/2002/07/owl
rdf => http://www.w3.org/1999/02/22-rdf-syntax-ns
rdfs => http://www.w3.org/2000/01/rdf-schema
brick => http://buildsys.org/ontologies/Brick
```

The Brick `ttl` files are loaded implicitly when HodDB is invoked, using the paths provided in the configuration file.

Loading is idempotent, meaning the same `.ttl` file can be loaded many times without affecting the performance or correctness of the database.

See [the examples page](/examples) for a small TTL file sample.

*Note*: if more than one building is loaded into the same HodDB instance, then the results from the two buildings will be returned together and there is the possibility
for namespace conflicts. Thus, at this early stage in HodDB development it is recommended to have a different HodDB instance for each building. Disk space is negligible: the Soda Hall Brick file has 3774 triples in it, and the on-disk representation only uses 1.5 MB.

*Note*: right now, HodDB only supports loading in buildings via the command line. Future versions may allow loading buildings over HTTP.

<a name="cliqueries"></a>
### CLI Queries

HodDB offers a command-line query interface which has the ability of exposing additional performance information as well as query planner details.
The visibility of this information is configurable in the HodDB configuration file.

By default, the command-line query interface can be invoked using the `hod cli` command:

```
hod cli
Successfully loaded dataset!
(hod)> SELECT ?floor ?room ?zone
>>> ...WHERE {
>>> ...    ?floor rdf:type brick:Floor .
>>> ...    ?room rdf:type brick:Room .
>>> ...    ?zone rdf:type brick:HVAC_Zone .
>>> ...    ?room bf:isPartOf+ ?floor .
>>> ...    ?room bf:isPartOf+ ?zone .
>>> ...};
INFO query.go:58 Dec 24 15:35:51  ▶ Ran query in 16.58933ms
INFO query.go:64 Dec 24 15:35:51  ▶ Expanded tuples in 10.785491ms
NOTICE api.go:60 Dec 24 15:35:51  ▶ Full Query took 28.42414ms
map[?zone:http://buildsys.org/ontologies/building_example#hvac_zone_C180 ?floor:http://buildsys.org/ontologies/building_example#floor_1 ?room:http://buildsys.org/ontologies/building_example#room_C180]
map[?floor:http://buildsys.org/ontologies/building_example#floor_1 ?room:http://buildsys.org/ontologies/building_example#room_R181 ?zone:http://buildsys.org/ontologies/building_example#hvac_zone_R181]
map[?room:http://buildsys.org/ontologies/building_example#room_R184 ?zone:http://buildsys.org/ontologies/building_example#hvac_zone_R184 ?floor:http://buildsys.org/ontologies/building_example#floor_1]
map[?floor:http://buildsys.org/ontologies/building_example#floor_1 ?room:http://buildsys.org/ontologies/building_example#room_R187A ?zone:http://buildsys.org/ontologies/building_example#hvac_zone_R187A]
map[?zone:http://buildsys.org/ontologies/building_example#hvac_zone_R190A ?floor:http://buildsys.org/ontologies/building_example#floor_1 ?room:http://buildsys.org/ontologies/building_example#room_R190A]
map[?floor:http://buildsys.org/ontologies/building_example#floor_1 ?room:http://buildsys.org/ontologies/building_example#room_R190B ?zone:http://buildsys.org/ontologies/building_example#hvac_zone_R190B]
map[?room:http://buildsys.org/ontologies/building_example#room_R252 ?zone:http://buildsys.org/ontologies/building_example#hvac_zone_R252 ?floor:http://buildsys.org/ontologies/building_example#floor_2]
map[?floor:http://buildsys.org/ontologies/building_example#floor_2 ?room:http://buildsys.org/ontologies/building_example#room_R283E ?zone:http://buildsys.org/ontologies/building_example#hvac_zone_R283E]
map[?zone:http://buildsys.org/ontologies/building_example#hvac_zone_R287 ?floor:http://buildsys.org/ontologies/building_example#floor_2 ?room:http://buildsys.org/ontologies/building_example#room_R287]
...
```

*Note*: only a single HodDB instance can be started for a set of backing files, i.e. you cannot run both the CLI and HTTP interface simultaneously.

<a name="cliconfigs"></a>
#### Config: `ShowDependencyGraph`

(Defaults to `false`)

If `true`, then HodDB will print out the dependency graph generated from the provided query, which is used as input into the query planner.

```
hod cli
Successfully loaded dataset!
(hod)> SELECT ?floor ?room ?zone
>>> ...WHERE {
>>> ...    ?floor rdf:type brick:Floor .
>>> ...    ?room rdf:type brick:Room .
>>> ...    ?zone rdf:type brick:HVAC_Zone .
>>> ...    ?room bf:isPartOf+ ?floor .
>>> ...    ?room bf:isPartOf+ ?zone .
>>> ...};
 <?zone [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#HVAC_Zone>
 <?room [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#Room>
   <?room [{http://buildsys.org/ontologies/BrickFrame#isPartOf +}] ?zone>
 <?floor [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#Floor>
   <?room [{http://buildsys.org/ontologies/BrickFrame#isPartOf +}] ?floor>
INFO query.go:58 Dec 24 15:43:33  ▶ Ran query in 16.422681ms
INFO query.go:64 Dec 24 15:43:33  ▶ Expanded tuples in 10.002746ms
NOTICE api.go:60 Dec 24 15:43:33  ▶ Full Query took 31.336022ms
```

#### Config: `ShowQueryPlan`

(Defaults to `false`)

If `true`, then HodDB will print out the generated query plan for the provided query, showing which low-level operations are being run over the underlying structure.

```
hod cli
Successfully loaded dataset!
(hod)> SELECT ?floor ?room ?zone
>>> ...WHERE {
>>> ...    ?floor rdf:type brick:Floor .
>>> ...    ?room rdf:type brick:Room .
>>> ...    ?zone rdf:type brick:HVAC_Zone .
>>> ...    ?room bf:isPartOf+ ?floor .
>>> ...    ?room bf:isPartOf+ ?zone .
>>> ...};
-------------- start query plan -------------
DEBUG query.go:45 Dec 24 15:47:00  ▶ op [resolveSubject <?floor [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#Floor>]
DEBUG query.go:45 Dec 24 15:47:00  ▶ op [resolveSubject <?room [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#Room>]
DEBUG query.go:45 Dec 24 15:47:00  ▶ op [resolveSubject <?zone [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#HVAC_Zone>]
DEBUG query.go:45 Dec 24 15:47:00  ▶ op [restrictSubObjByPred <?room [{http://buildsys.org/ontologies/BrickFrame#isPartOf +}] ?floor>]
DEBUG query.go:45 Dec 24 15:47:00  ▶ op [restrictSubObjByPred <?room [{http://buildsys.org/ontologies/BrickFrame#isPartOf +}] ?zone>]
-------------- end query plan -------------
INFO query.go:58 Dec 24 15:47:00  ▶ Ran query in 26.436114ms
INFO query.go:64 Dec 24 15:47:00  ▶ Expanded tuples in 7.149802ms
NOTICE api.go:60 Dec 24 15:47:00  ▶ Full Query took 35.000475ms
```

#### Config: `ShowQueryPlanLatencies`

(Defaults to `false`)

If `true`, then HodDB will print out the time to execute each step of the query plan, which can be helpful for debugging slow queries.

```
hod cli
Successfully loaded dataset!
(hod)> SELECT ?floor ?room ?zone
>>> ...WHERE {
>>> ...    ?floor rdf:type brick:Floor .
>>> ...    ?room rdf:type brick:Room .
>>> ...    ?zone rdf:type brick:HVAC_Zone .
>>> ...    ?room bf:isPartOf+ ?floor .
>>> ...    ?room bf:isPartOf+ ?zone .
>>> ...};
INFO query.go:49 Dec 24 15:49:04  ▶ Formed execution plan in 47.007µs
[resolveSubject <?floor [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#Floor>] 201.308µs
[resolveSubject <?room [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#Room>] 416.97µs
[resolveSubject <?zone [{http://www.w3.org/1999/02/22-rdf-syntax-ns#type }] http://buildsys.org/ontologies/Brick#HVAC_Zone>] 386.676µs
[restrictSubObjByPred <?room [{http://buildsys.org/ontologies/BrickFrame#isPartOf +}] ?floor>] 8.29247ms
[restrictSubObjByPred <?room [{http://buildsys.org/ontologies/BrickFrame#isPartOf +}] ?zone>] 10.253657ms
INFO query.go:58 Dec 24 15:49:04  ▶ Ran query in 19.647705ms
INFO query.go:64 Dec 24 15:49:04  ▶ Expanded tuples in 7.858543ms
NOTICE api.go:60 Dec 24 15:49:04  ▶ Full Query took 28.793678ms
```

#### Config: `ShowQueryLatencies`

Defaults to `true`; shows the runtime, tuple-expansion and overall total time to execute the query. Note that this is slightly more accurate than the times shown in the HTTP interface because those involve network and browser overhead.

<a name="httpapi"></a>
### HTTP API

The HodDB HTTP server can be started on the command line via:

```bash
hod http
NOTICE server.go:78 Dec 24 17:28:53  ▶ Starting HTTP Server on  localhost:47808
```

The served port can be configured using the `ServerPort` config option. IPv6 and public/localhost serving are configured via the `UseIPv6` and `Localhost` fields respectively.

The exposed endpoints are:

- `/api/query`: receives POST requests containing a query string, returns JSON encoded results of the query
- `/api/loadlinks`: receives POST requests containing a JSON-encoded links file. Returns 200 or error.
- `/api/querydot`: receives POST requests containing a query string, returns a GraphViz compatible DOT representation of the query as a string.


<a name="httpfront"></a>
### HTTP Frontend

There is a prototype HodDB interactive query interface available at the base URI, [http://localhost:47808](http://localhost:47808).
It has its own set of documentation at [http://localhost:47808/help](http://localhost:47808/help).

<a name="bosswaveapi"></a>
### BOSSWAVE API

There is a prototype HodDB interactive query interface available at the base URI, [http://localhost:47808](http://localhost:47808).
It has its own set of documentation at [http://localhost:47808/help](http://localhost:47808/help).

<a name="tools"></a>
### Other Tools

The `hod` tool has a collection of other tools that may be useful. Use the `-h` flag on any of these to get more documentation on their usage.

- `hod dumpgraph`: creates a PDF visualization of the nodes and entities in a TTL file. Warning! This can get very big and intractable.

- `hod dump`: prints out the contents of a TTL file as a set of tab-separated triples

- `hod benchload`: benchmarks the parsing time of a specified TTL file

