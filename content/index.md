---
title: HodDB
type: homepage
menu: main
weight: 0
---

## A Database for Brick

HodDB is a specialized RDF/SPARQLl triplestore database for [Brick](http://brickschema.org/). 
It stores models of buildings and serves queries on those models.

HodDB has the following goals:

* sub-100ms SPARQL queries on Brick graphs 
* facilitate integration with external services:
    * use Literals to store timeseries UUIDs or application URIs
* [BOSSWAVE](https://github.com/immesys/bw2) integration
* store the history of changes to the Brick model of the building
    * (currently under development)

### Quickstart

From source:

```bash
$ sudo apt install libraptor2-dev graphviz
$ go get github.com/gtfierro/hod
$ go install github.com/gtfierro/hod
$ hod load path/to/building.ttl
$ hod server
NOTICE server.go:84 Dec 28 21:37:56  ▶ Starting HTTP Server on  0.0.0.0:47808
```

From Docker:

```bash
$ sudo apt install docker.io # if necessary
$ docker pull gtfierro/hoddb
$ docker run -d --name hoddb -v /etc/hod:/etc/hod -p 47808:47808 gtfierro/hoddb
```

### Public Query Server

Available at [https://query.hoddb.org/](https://query.hoddb.org/)
