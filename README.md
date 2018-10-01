[![CircleCI](https://circleci.com/gh/bitnami/bitnami-docker-fluentd/tree/master.svg?style=shield)](https://circleci.com/gh/bitnami/bitnami-docker-fluentd/tree/master)

# What is Fluentd?

Fluentd is an open source data collector, which lets you unify the data collection and consumption for a better use and understanding of data.
[https://fluentd.org](https://fluentd.org)

# TL;DR;

```bash
$ docker run --name fluentd bitnami/fluentd:latest
```

# Why use Bitnami Images?

* Bitnami closely tracks upstream source changes and promptly publishes new versions of this image using our automated systems.
* With Bitnami images the latest bug fixes and features are available as soon as possible.
* Bitnami containers, virtual machines and cloud images use the same components and configuration approach - making it easy to switch between formats based on your project needs.
* Bitnami images are built on CircleCI and automatically pushed to the Docker Hub.
* All our images are based on [minideb](https://github.com/bitnami/minideb) a minimalist Debian based container image which gives you a small base container image and the familiarity of a leading linux distribution.

# Why use a non-root container?

Non-root container images add an extra layer of security and are generally recommended for production environments. However, because they run as a non-root user, privileged tasks are typically off-limits. Learn more about non-root containers [in our docs](https://docs.bitnami.com/containers/how-to/work-with-non-root-containers/).

# Supported tags and respective `Dockerfile` links

> NOTE: Debian 8 images have been deprecated in favor of Debian 9 images. Bitnami will not longer publish new Docker images based on Debian 8.

Learn more about the Bitnami tagging policy and the difference between rolling tags and immutable tags [in our documentation page](https://docs.bitnami.com/containers/how-to/understand-rolling-tags-containers/).


* [`1-ol-7`, `1.2.5-ol-7-r34` (1/ol-7/Dockerfile)](https://github.com/bitnami/bitnami-docker-fluentd/blob/1.2.5-ol-7-r34/1/ol-7/Dockerfile)
* [`1-debian-9`, `1.2.5-debian-9-r33`, `1`, `1.2.5`, `1.2.5-r33`, `latest` (1/debian-9/Dockerfile)](https://github.com/bitnami/bitnami-docker-fluentd/blob/1.2.5-debian-9-r33/1/debian-9/Dockerfile)

Subscribe to project updates by watching the [bitnami/fluentd GitHub repo](https://github.com/bitnami/bitnami-docker-fluentd).

# Get this image

The recommended way to get the Bitnami Fluentd Docker Image is to pull the prebuilt image from the [Docker Hub Registry](https://hub.docker.com/r/bitnami/fluentd).

```bash
$ docker pull bitnami/fluentd:latest
```

To use a specific version, you can pull a versioned tag. You can view the [list of available versions](https://hub.docker.com/r/bitnami/fluentd/tags/) in the Docker Hub Registry.

```bash
$ docker pull bitnami/fluentd:[TAG]
```

If you wish, you can also build the image yourself.

```bash
$ docker build -t bitnami/fluentd:latest https://github.com/bitnami/bitnami-docker-fluentd.git
```

# Connecting to other containers

Using [Docker container networking](https://docs.docker.com/engine/userguide/networking/), a different server running inside a container can easily be accessed by your application containers and vice-versa.

Containers attached to the same network can communicate with each other using the container name as the hostname.

## Using the Command Line

### Step 1: Create a network

```bash
$ docker network create fluentd-network --driver bridge
```

### Step 2: Launch the Blacbox_exporter container within your network

Use the `--network <NETWORK>` argument to the `docker run` command to attach the container to the `fluentd-network` network.

```bash
$ docker run --name fluentd-node1 --network fluentd-network bitnami/fluentd:latest
```

### Step 3: Run another containers

We can launch another containers using the same flag (`--network NETWORK`) in the `docker run` command. If you also set a name to your container, you will be able to use it as hostname in your network.


# Configuration

To create endpoint that collectc logs on your host just run:

```
docker run -d -p 24224:24224 -p 24224:24224/udp -v /data:/opt/bitnami/fluentd/log fluentd
```

Default configurations are to:

 - configuration file at `/opt/bitnami/fluentd/conf/fluentd.conf`
 - listen port `24224` for Fluentd forward protocol
 - store logs with tag `docker.**` into `/opt/bitnami/fluentd/log/docker.*.log`
 - store all other logs into `/opt/bitnami/fluentd/log/data.*.log`

You can overwrite the default configuration file by mounting your own configuration file on the directory `/opt/bitnami/fluentd/conf`:

```
docker run --name fluentd -v /path/to/fluentd.conf:/opt/bitnami/fluentd/conf/fluentd.conf bitnami/fluentd:latest
```

Using Docker Compose:

```yaml
version: '2'

services:
  fluentd:
    image: bitnami/fluentd:latest
    ports:
      - '24224:24224'
      - '5140:5140'
    volumes:
      - /path/to/fluentd.conf:/opt/bitnami/fluentd/conf/fluentd.conf
```

You can also extend the default configuration by importing your custom configuration with the "@include" directive. It is a simple as creating a directory with you custom config files and mount it on the directory `/opt/bitnami/fluentd/conf/conf.d`:

```
docker run --name fluentd -v /path/to/custom-conf-directory:/opt/bitnami/fluentd/conf/conf.d bitnami/fluentd:latest
```

Using Docker Compose:

```yaml
version: '2'

services:
  fluentd:
    image: bitnami/fluentd:latest
    ports:
      - '24224:24224'
      - '5140:5140'
    volumes:
      - /path/to/custom-conf-directory:/opt/bitnami/fluentd/conf/conf.d
```

Find more information about this feature, consult [official documentation](https://docs.fluentd.org/v0.12/articles/config-file)

# Environment Variables

Environment variable below are configurable to control how to execute fluentd process:

 - `FLUENTD_CONF`: This variable allows you to specify configuration file name that will be used in -c Fluentd command line option. If you want to use your own configuration file (without any optional plugins), you can do it with this environment variable and Docker volumes (-v option of docker run).

 - `FLUENTD_OPT`: Use this variable to specify other Fluentd command line options, like -v or -q.

# Logging

The Bitnami fluentd Docker image sends the container logs to the `stdout`. To view the logs:

```bash
$ docker logs fluentd
```

You can configure the containers [logging driver](https://docs.docker.com/engine/admin/logging/overview/) using the `--log-driver` option if you wish to consume the container logs differently. In the default configuration docker uses the `json-file` driver.

# Maintenance

## Upgrade this image

Bitnami provides up-to-date versions of fluentd, including security patches, soon after they are made upstream. We recommend that you follow these steps to upgrade your container.

### Step 1: Get the updated image

```bash
$ docker pull bitnami/fluentd:latest
```

### Step 2: Stop and backup the currently running container

Stop the currently running container using the command

```bash
$ docker stop fluentd
```

Next, take a snapshot of the persistent volume `/path/to/fluentd-persistence` using:

```bash
$ rsync -a /path/to/fluentd-persistence /path/to/fluentd-persistence.bkp.$(date +%Y%m%d-%H.%M.%S)
```

You can use this snapshot to restore the database state should the upgrade fail.

### Step 3: Remove the currently running container

```bash
$ docker rm -v fluentd
```

### Step 4: Run the new image

Re-create your container from the new image, [restoring your backup](#restoring-a-backup) if necessary.

```bash
$ docker run --name fluentd bitnami/fluentd:latest
```

# Contributing

We'd love for you to contribute to this container. You can request new features by creating an [issue](https://github.com/bitnami/bitnami-docker-fluentd/issues), or submit a [pull request](https://github.com/bitnami/bitnami-docker-fluentd/pulls) with your contribution.

# Issues

If you encountered a problem running this container, you can file an [issue](https://github.com/bitnami/bitnami-docker-fluentd/issues). For us to provide better support, be sure to include the following information in your issue:

- Host OS and version
- Docker version (`docker version`)
- Output of `docker info`
- Version of this container
- The command you used to run the container, and any relevant output you saw (masking any sensitive information)

# License
Copyright 2018 Bitnami

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
