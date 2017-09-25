## Docker HBase Standalone Dockerfile

This repository contains **Dockerfile** of [Hbase 1.1.12](http://hbase.apache.org/) for [Docker](https://www.docker.com/) [automated build](https://registry.hub.docker.com/u/cogniteev/hbase-standalone/) published to the public [Docker Hub Registry](https://registry.hub.docker.com/).

### Base Docker Image

* [dockerfile/java:oracle-java8](http://dockerfile.github.io/#/java)

### Installation

1. Install [Docker](https://www.docker.com/)

2. Download [automated build](https://registry.hub.docker.com/u/cogniteev/hbase-standalone/): `docker pull cogniteev/hbase-standalone`

### Basic usage

```sh
docker run -d -p 2181:2181 -p 60000:60000 -p 60010:60010 -p 60020:60020 -p 60030:60030 cogniteev/hbase-standalone
```

Open http://docker.ip.add.ress:60010 in a browser

### Client configuration

HBase client must connect using the hbase hostname, ip address is not enough. By default, hostname is assigned randomly by Docker (it is the container identifier). Here is one method so that you other container can talk to hbase-standalone:


```sh
$ docker run -d \
    -p 41189:41189 \
    -p 2181:2181 \
    -p 60000:60000 \
    -p 60010:60010 \
    -p 60020:60020 \
    -p 60030:60030 \
    -h hbase-srv \
    cogniteev/hbase-standalone
72531260512e416e2d7bf3bb452917972df1328f86ab4b6fbdb476222b009363
$ docker run 
    --link \
    72531260512e416e2d7bf3bb452917972df1328f86ab4b6fbdb476222b009363:hbase-srv 
    hase/client-container
```

In client-container, you can then connect to hbase using **hbase-srv**

### Attach persistent/shared directories

Hbase data are stored in */home/hbase/data*. Logs are stored in */home/hbase/logs*.

For instance, to bind <data-dir> on the host, start the container with the following volume:
```sh
docker run -d -v <data-dir>:/home/hbase/data cogniteev/hbase-standalone
```

### Notes

This container runs the exact version distributed by Apache.
