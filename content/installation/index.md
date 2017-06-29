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

If you have [Docker installed](https://docs.docker.com/engine/installation/), then HodDB can be started by first pulling the latest `gtfierro/hoddb` Docker image and then starting it with the appropriate parameters.

```bash
$ docker pull gtfierro/hoddb
$ docker run -d --name hoddb -v /etc/hod:/etc/hod -p 47808:47808 gtfierro/hoddb
```

The docker container expects `hodconfig.yml` (configuration file) and `building.ttl` (Brick model) to be in the `/etc/hod` directory.

The HTTP interface will be available at [http://localhost:47808](http://localhost:47808); the exposed port is configurable via the `docker run` command above.

To build your own Docker image, use the template [provided here](https://github.com/gtfierro/brick_database_eval/tree/master/hod), adjusting `build.sh` and `Dockerfile` to load the correct `.ttl` file representing the building.

As HodDB matures it will be possible to load buildings over the HTTP interface, but this is not currently supported.

<a name="binary"></a>
### Binary

To have slightly more control over the installation, a binary and configuration file can be downloaded from [GitHub](https://github.com/gtfierro/hod/releases/latest).

This will require the installation of the [Raptor RDF Syntax Library](http://librdf.org/raptor/) and GraphViz, which can be done over `apt`:

```bash
$ sudo apt install libraptor2-dev graphviz
```

After unpacking the archive, HodDB can be started via the command line:

```bash
# NOTE this output may change slightly
$ ./hod
NAME:
   hod - BRICK database and query engine

USAGE:
   hod [global options] command [command options] [arguments...]

VERSION:
   0.4.3

COMMANDS:
     benchload  Benchmark loading a turtle file
     dump       Dump contents of turtle file
     viewclass  PDF visualization of class structure of file
     dumpgraph  PDF visualization of TTL file. WARNING this can get really big
     load       Load dataset into hoddb
     loadLinks  Load link json file into hoddb
     cli        Start hoddb from existing database
     http       Start hoddb HTTP server from existing database
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
$ go get -u github.com/gtfierro/hod
$ go install github.com/gtfierro/hod
```

After this, then HodDB can be executed on the command line:

```bash
# NOTE this output may change slightly
$ hod
NAME:
   hod - BRICK database and query engine

USAGE:
   hod [global options] command [command options] [arguments...]

VERSION:
   0.2

COMMANDS:
     benchload  Benchmark loading a turtle file
     dump       Dump contents of turtle file
     viewclass  PDF visualization of class structure of file
     dumpgraph  PDF visualization of TTL file. WARNING this can get really big
     load       Load dataset into hoddb
     loadLinks  Load link json file into hoddb
     cli        Start hoddb from existing database
     http       Start hoddb HTTP server from existing database
     help, h    Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --help, -h     show help
   --version, -v  print the version
```