---
menu: main
title: Installation
weight: 1
---

- [Docker](#docker)
- [Binary](#binary)
- [Source](#source)

<a name="docker"></a>
### Docker

If you have [Docker installed](https://docs.docker.com/engine/installation/), then HodDB can be started by pulling the `gtfierro/hod:latest` Docker image and then starting it with the appropriate parameters.
HodDB requires a directory containing a configuration file and the Brick models. When we start the Docker container, we need to mount this directory (referred to as `configdir` below):

```bash
$ docker pull gtfierro/hod:latest
# you can change 'configdir' to whatever absolute path
$ docker run -d --name hod -v `pwd`/configdir:/etc/hod -p 47808:47808 gtfierro/hod:latest
```

You should place the following inside the directory that gets mounted to `/etc/hod`

- `hodconfig.yaml`: the configuration file for HodDB
- the Brick models (`.ttl` files) to be loaded into HodDB

See [the configuration documentation](/configuration) for how to populate these files.


The HTTP interface will be available at [http://localhost:47808](http://localhost:47808); the exposed port is configurable via the `docker run` command above (change the second `47808`).


<a name="binary"></a>
### Binary

To have slightly more control over the installation, a binary and configuration file can be downloaded from [GitHub](https://github.com/gtfierro/hod/releases/latest).

This will require the installation of the [Raptor RDF Syntax Library](http://librdf.org/raptor/) and GraphViz, which can be done using `apt`:

```bash
$ sudo apt install libraptor2-dev graphviz
```

After unpacking the archive, HodDB can be started via the command line:

```bash
# NOTE this output may change slightly
$ ./hod
 _    _           _ _____  ____ 
| |  | |         | |  __ \|  _ \  
| |__| | ___   __| | |  | | |_) | 
|  __  |/ _ \ / _` | |  | |  _ <  
| |  | | (_) | (_| | |__| | |_) | 
|_|  |_|\___/ \__,_|_____/|____/  
Commit: 71deeb3   Release: 0.5.2

NAME:
   hod - BRICK database and query engine

USAGE:
   hod [global options] command [command options] [arguments...]

VERSION:
   0.5.2

COMMANDS:
     cli        Start hoddb from existing database
     server     Start hoddb server from existing database. Default to HTTP server only, but can do both that and BOSSWAVE
     query      Query from command line (non-interactive)
     benchload  Benchmark loading a turtle file
     dump       Dump contents of turtle file
     viewclass  PDF visualization of class structure of file
     dumpgraph  PDF visualization of TTL file. WARNING this can get really big
     ttlstat    Outputs statistics on the provided TTL file. Loads all file provided as arguments
     help, h    Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --help, -h     show help
   --version, -v  print the version
```

You may see an error print out looking something like

```
./hod: /usr/local/lib/libraptor2.so.0: no version information available (required by ./hod)
```

but HodDB should perform normally. Please [file an issue](https://github.com/gtfierro/hod/issues) if you notice anything.

Note: current releases are just for 64-bit Linux. This will change as HodDB matures.

<a name="source"></a>
### Source

HodDB can also be installed from source. This method is good for developing HodDB, or alternatively staying up to date with the "bleeding edge", which is probably only good if you need the latest fixes and can't wait for an official release.

To install/update HodDB, you need [Go](https://golang.org/doc/install) installed and your `$GOPATH` and `$GOROOT` environment variables configured (how to set these is discussed in the Go installation guide), as well as `$GOPATH/bin` added to your `$PATH`.

```bash
$ sudo apt install libraptor2-dev graphviz
$ go get -u github.com/gtfierro/hod
$ go install github.com/gtfierro/hod
```

After this, then HodDB can be executed on the command line:

```bash
# NOTE this output may change slightly
$ hod
 _    _           _ _____  ____ 
| |  | |         | |  __ \|  _ \  
| |__| | ___   __| | |  | | |_) | 
|  __  |/ _ \ / _` | |  | |  _ <  
| |  | | (_) | (_| | |__| | |_) | 
|_|  |_|\___/ \__,_|_____/|____/  
Commit: 71deeb3   Release: 0.5.2

NAME:
   hod - BRICK database and query engine

USAGE:
   hod [global options] command [command options] [arguments...]

VERSION:
   0.5.2

COMMANDS:
     cli        Start hoddb from existing database
     server     Start hoddb server from existing database. Default to HTTP server only, but can do both that and BOSSWAVE
     query      Query from command line (non-interactive)
     benchload  Benchmark loading a turtle file
     dump       Dump contents of turtle file
     viewclass  PDF visualization of class structure of file
     dumpgraph  PDF visualization of TTL file. WARNING this can get really big
     ttlstat    Outputs statistics on the provided TTL file. Loads all file provided as arguments
     help, h    Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --help, -h     show help
   --version, -v  print the version
```

