---
title: HodDB
type: homepage
menu: main
weight: 0
---

## A Database for Brick

HodDB is a database for storing [Brick](http://brickschema.org/) models of buildings and serving queries on those models. More generally, it is an RDF triplestore with limited reasoning support, but many other features.

HodDB has the following goals:

* sub-100ms SPARQL queries on small graphs (buildings are typically < 100,000 triples)
* facilitate integration with external services:
    * attach key-value "links" such as timeseries UUIDs or application URIs
* store the history of changes to the Brick model of the building
    * (currently under development)

### Quickstart

```bash
$ sudo apt install libraptor2-dev graphviz
$ go get github.com/gtfierro/hod
$ go install github.com/gtfierro/hod
$ hod load path/to/building.ttl
$ hod http
NOTICE server.go:84 Dec 28 21:37:56  ▶ Starting HTTP Server on  0.0.0.0:47808
```

### Public Query Server

Available at [http://query.hoddb.org/](http://query.hoddb.org/)
