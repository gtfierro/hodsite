---
menu: main
title: Using HodDB
weight: 3
---

### Loading Data

#### Overview

In Brick, buildings are described using RDF triples. These triples are stored in [Turtle (`.ttl`) files](https://www.w3.org/TR/turtle/), which can be loaded into a HodDB instance.

HodDB stores buildings in a set of LevelDB databases; the location of these files is determined by the `DBPath` configuration option which defaults to `./_hoddb`.

In order to perform queries on the the database using the Brick classes and relationships, HodDB also needs to import the Turtle files that define the Brick schema. The location of these files are determined by the `BrickFrameTTL` and `BrickClassTTL` configuration options. The most recent version of these files can be downloaded from [http://brickschema.org/download/](http://brickschema.org/download/).

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

#### Links File

HodDB supports "links" which are simple string key-value pairs that act as pointers to external resources. Brick stores a representation of the structure and relationships for building subsystems, but only stores the edges and nodes in that graph.

In a building operating system, there may be a set of associated resources for each "entity" in the building graph:

- UUID for associated timeseries streams
- Link to a driver implementation
- Link to a UI for the entity
- Link to documentation for the entity
- etc.

Rather than storing this information using RDF triples which would be verbose and cumbersome, HodDB uses a separate data model for storing such "links".

Links files use the full URIs of the graph entities, so make sure to note the building prefix when creating the links file.

Links files are JSON and follow this structure:

```json
{
	// how to add 2 links to an entity
	"<full uri of building entity>": {
		"<link name>": "<link value>",
		"<link name>": "<link value>"
	},
	// deleting one link and adding another
	"<full uri of building entity>": {
		"<link name>": "",
		"<link name>": "<link value>"
	},
	// deleting all links for a given entity
	"<full uri of building entity>": {}
}
```

For example:

```json
{
    "http://buildsys.org/ontologies/building_example#temp_sensor_hvac_zone_R739": {
        "UUID": "3a038c7c-c7e6-11e6-bfc1-1002b58053c7"
    }
}
```

This associates the given UUID for a timeseries to the temperature sensor for Room 739 in the example building.

#### Loading Links

In order to load a JSON links file, we use the `hod loadLinks` command:

```bash
$ hod loadLinks links.json
NOTICE actions.go:89 Dec 24 14:26:25  ▶ Adding 10 links, Removing 0 links
```

### CLI Queries

### HTTP API

### HTTP Frontend

### Other Tools
