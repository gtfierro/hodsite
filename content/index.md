---
title: HodDB
type: homepage
menu: main
weight: 0
---

## A Database for Brick

HodDB is a specialized RDF database and SPARQL query processor for [Brick](http://brickschema.org/). 
It stores models of buildings and serves queries on those models.

HodDB has the following features:

* sub-100ms SPARQL queries on Brick graphsmodels
* web interface for interacting with Brick models
* [BOSSWAVE](https://github.com/immesys/bw2) integration
* integration with external services:
    * uses Literals to store timeseries UUIDs or application URIs


### Quickstart

From Docker:

```bash
$ sudo apt install docker.io # install docker if necessary
$ curl -L https://github.com/gtfierro/hod/releases/download/v0.6.0/quickstart.tar.gz | tar xzv
$ docker run -d --name hoddb -v $PWD/quickstart:/etc/hod -p 47808:47808 gtfierro/hoddb
```

Then navigate to [http://localhost:47808/query](http://localhost:47808/query)

From source:

```bash
# requires go >= 1.10 installed
$ sudo apt install libraptor2-dev graphviz
$ go get github.com/gtfierro/hod
$ cd $GOPATH/src/github.com/gtfierro/hod
$ make install
$ cd quickstart
$ hod server
NOTICE server.go:84 Dec 28 21:37:56  ▶ Starting HTTP Server on  0.0.0.0:47808
```

### Public Query Server

~~available at [https://query.hoddb.org/](https://query.hoddb.org/)~~ currently unavailable
