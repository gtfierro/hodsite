---
menu: main
title: Configuration
weight: 2
---

HodDB is configured with a YAML file. By default, this file is called `hodconfig.yml` and is loaded from the current directory, though this can be changed with the `--config/-c` command line option when executing the `hod` binary. All paths below are relative to the location from which hod is executed.

Configuration defaults are as follows; there's usually no reason to change these aside from the list of buildings to load and the network configuration.

```yaml
### HodDB Configuration
# commented out lines are default configuration options

####
# Location and structure of database
####

# how we load data into the database.
# The keys here are the names of the graphs and how they are referred to when
# querying. The values are file paths to the source for each graph
Buildings:
    ciee: buildings/ciee.ttl
    sdh: buildings/sdh.ttl
    soda: buildings/soda.ttl

# the location of the database files
DBPath: _hoddb

Ontologies:
- "$GOPATH/src/github.com/gtfierro/hod/BrickFrame.ttl"
- "$GOPATH/src/github.com/gtfierro/hod/Brick.ttl"

####
# Interface Enabling
####
#
# Enable HTTP server
EnableHTTP: true
#
# Enable BOSSWAVE server
EnableBOSSWAVE: false

####
# HTTP Server Configuration
####

# port to run the server on
ServerPort: 47808

# Whether or not to serve on IPv6
UseIPv6: false

# Whether or not to serve on localhost. If false, serves on a public interface
ListenAddress: 0.0.0.0

# If specified, serve the frontend over HTTPS using golang.org/x/crypto/acme/autocert
# If left blank (default), just serve over HTTP
TLSHost: ""

####
# BOSSWAVE Server Configuration
####

# BOSSWAVE agent
BW2_AGENT: $BW2_AGENT

# BOSSWAVE entity
BW2_DEFAULT_ENTITY: $BW2_DEFAULT_ENTITY

# Base URI
HodURI: scratch.ns/hod

####
# configuration for verbosity during operation
####
#
# Show the namespace prefixes
ShowNamespaces: false

# Show the built dependency graph of query terms
ShowDependencyGraph: false

# Show the set of operations in the query plan
ShowQueryPlan: false

# Show the latencies of creating the query plan
ShowQueryPlanLatencies: false

# Show the latencies of each operation in the query plan
ShowOperationLatencies: false

# Show the full latency of the query (and its larger components)
ShowQueryLatencies: false

# Set log level. In order of increasing verbosity:
# Debug, Info, Notice, Warning, Error, Critical
LogLevel: Error

####
# Profiling Information for HTTP Server
####
# Note that best practices dictate that these be done in a mutually exclusive
# manner, e.g. do not enable CPU and MEM profiling at the same time because
# they will end up measuring the effect of measuring
# Also note that if you are trying to use the net/http/profile trick for generating
# torch graphs, you will need to keep these both at false

# Enable CPU profile
EnableCPUProfile: false

# Enable MEM profile
EnableMEMProfile: false

# Enable Block profile
EnableBlockProfile: false
```

#### Example

Here is a brief example of how to set up the configuration.

The directory 'sample' has the `hodconfig.yaml` file above. We create a folder called `buildings` to store our 3 TTL files representing 3 Brick models. This is indicted to HodDB
in the `Buildings` configuration option above.

```bash
$ tree
.
├── buildings
│   ├── ciee.ttl
│   ├── sdh.ttl
│   └── soda.ttl
└── hodconfig.yaml
```

We can then execute HodDB against this configuration with

```bash
$ docker run -d --name hod -v `pwd`/sample:/etc/hod -p 47808:47808 gtfierro/hod:latest
```

You can then stop/start HodDB using

```bash
$ docker stop hod
$ docker start hod
```

and remove its data with

```bash
$ docker kill hod
$ docker rm hod
```
