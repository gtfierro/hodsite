---
menu: main
title: Configuration
weight: 2
---

HodDB is configured with a YAML file. By default, this file is called `hodconfig.yml` and is loaded from the current directory, though this can be changed with the `--config/-c` command line option when executing the `hod` binary.

Configuration defaults are as follows; there's usually no reason to change these aside from the network configuration.


```yaml
### HodDB Configuration
# commented out lines are default configuration options

####
# Location and structure of database
####

# the location of the database files
#DBPath: _hoddb

# the path to the TTL file containing Brick relationships
#BrickFrameTTL: "$GOPATH/src/github.com/gtfierro/hod/BrickFrame.ttl"

# the path to the TTL file containing Brick classes
#BrickClassTTL: "$GOPATH/src/github.com/gtfierro/hod/Brick.ttl"

# whether or not to reload the Brick database files
#ReloadBrick: true

# By default, each query w/n a generation is cached (up to 64 MB)
# If DisableQueryCache is flipped to true, then this cache is disabled
# and all queries run directly against the database
#DisableQueryCache: false

####
# Interface Enabling
####
#
# Enable HTTP server
#EnableHTTP: true
#
# Enable BOSSWAVE server
#EnableBOSSWAVE: false

####
# configuration for verbosity during operation
####
#
# Show the namespace prefixes
ShowNamespaces: true

# Show the built dependency graph of query terms
ShowDependencyGraph: false

# Show the set of operations in the query plan
ShowQueryPlan: false

# Show the latencies of creating the query plan
ShowQueryPlanLatencies: false

# Show the latencies of each operation in the query plan
ShowOperationLatencies: false

# Show the full latency of the query (and its larger components)
ShowQueryLatencies: true

# Set log level. In order of increasing verbosity:
# Debug, Info, Notice, Warning, Error, Critical
LogLevel: Error


####
# HTTP Server Configuration
####

# port to run the server on
ServerPort: 47808

# Whether or not to serve on IPv6
UseIPv6: false

# Whether or not to serve on localhost. If false, serves on a public interface
ListenAddress: 0.0.0.0

# Path to the server directory of hod, which contains the necessary HTML files
StaticPath: $GOPATH/src/github.com/gtfierro/hod/server

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