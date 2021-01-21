# Docker Command Reference 

[TOC]

[Top](#Docker-Command-Reference) [Next>](#Dockerfile)

## Docker Basics

### Docker Images



#### Pull from Docker Hub

```sh
docker image pull alpine:latest
```

#### Delete images

```sh
docker image rm testImage
docker image rm 095cf376b73a
```

#### Contents of a Docker image

```sh
docker image inspect alpine | less
```

There is a lot of information, so it can be confusing, but as an example look for the values for `Cmd` and `Env` meta-data. To check the required tags in the JSON output use:

```sh
docker image inspect alpine --format '{{.ContainerConfig.Cmd}}'
docker image inspect alpine --format '{{.ContainerConfig.Env}}'
```

### Container Life-cycle: Run/List/Stop/Remove

#### Run interactively

Multiple containers can be created from an image, therefore a **container** is an **active image** with a unique name/identity. Run a container **i**nteractively and handover control in a **t**erminal as:

```sh
docker container run -it --name myAlpine alpine
```

You can now type Linux shell commands at the container shell prompt. Type `exit` to stop the container.

If you run an image not pulled (cached) already, docker first pulls it from the hub and then runs a container from it. For example

```sh
docker image rm -f alpine
docker run -it --name myAlpine alpine
```

#### Stop

To stop a running container

```sh
docker container stop myAlpine
```

#### List

To list running containers

```sh
docker container ls -a
# or
docker container ps -a
```

#### Inspect

Various parameters of a running container can be viewed using `inspect` command

```sh
docker container inspect --format '{{.State.Health.Status}}' myAlpine
```

#### Log

log command can show us the logs of running applications in a container

```sh
docker container logs myAlpine
```

#### Remove

Remove a container

```sh
docker container rm myAlpine
```

Remove all stopped containers

```sh
docker container rm $(docc ps -a -q)
```

Remove all exited containers

```sh
docker container rm $(docc ps -q -f status=exited)
```

#### Add aliases

Frequent commands can be shortened by adding aliases to `~/.bashrc` file like

```sh
alias doci="docker image"
alias docc="docker container"
```

#### Run detached

Run a container in detached mode with a command run definitely/indefinitely

```sh
docc run -d --name myAlpine alpine /bin/sh -c 'while echo $(( i += 1)) ; do sleep 5 ; done'
```

This image will keep running in detached mode, printing out integers to `stdout` every five seconds. To check the state of the container

```sh
docc inspect --format {{.State.Status}} myAlpine
docc ls
```

#### Start a stopped container

Start the stopped container in `attached` mode

```sh
docc start -a myAlpine
```

#### Execute commands in a container

We can even execute a new process in the container.

```sh
docc exec -it myAlpine /bin/sh
```

#### Run with remove on exit

If we don’t need to access to the container after it’s stopped we can pass the `--rm` option to `docc run ...`, for example:

```sh
docc run -d --rm --name myAlpine alpine

# you can specify a custom coomand/program to run in the container
# and --rm option if you don't want to keep the container.
docc run --rm -it --name myAlpine alpine:latest /bin/sh
# examples:
docker container run --rm -it --name ubuntu.test ubuntu:latest /bin/bash
docker container run --rm -it --name test alpine:latest /bin/sh -c 'echo $HOME'
```

#### List dangling images

```sh
doci ls --filter dangling=true
```

#### Remove dangling images/containers/networks/volumes

```sh
docker container prune
docker image prune
docker network prune
docker volume prune
```

[<Prev](#Docker-Basics) [Top](#Docker-Command-Reference) [Next>](#Talking-to-the-outside-world)

## Dockerfile

Contents of this section are mostly borrowed from docker docs topics [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) and [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/). Hence, refer to these documents for a detailed explanation.

### Syntax



### `.dockerignore` file



### Instructions

#### `FROM`

#### `ADD`

#### `COPY`

#### `ENV`

#### `ARG`

The `ARG` instruction defines a variable that users can pass at build-time to the builder with the `docker build` command using the `--build-arg <varname>=<value>` flag. An `ARG` instruction can optionally include a default value, and if there is no value passed at build-time, the builder uses the default.

```dockerfile
FROM busybox
ARG user
USER ${USER:-user}
USER $USER
# ...
```

```sh
docker build --build-arg user=some_user .
```

An `ARG` instruction goes out of scope at the end of the build stage where it was defined. To use an arg in multiple stages, each stage must include the `ARG` instruction. An `ARG` instruction goes out of scope at the end of the build stage where it was defined. To use an arg in **multiple stages**, each stage must include the `ARG` instruction.

```dockerfile
FROM ubuntu
ARG SETTINGS
ARG CONT_IMG_VER
RUN ./run/setup $SETTINGS
ENV CONT_IMG_VER=${CONT_IMG_VER:-v1.0.0}
RUN echo $CONT_IMG_VER
# ...
FROM ubuntu
ARG SETTINGS
RUN ./run/setup $SETTINGS
```

#### `EXPOSE`

#### `LABEL`

#### `STOPSIGNAL`

#### `USER`

```dockerfile
USER <user>[:<group>]
```

or

```dockerfile
USER <UID>[:<GID>]
```

The `USER` instruction sets the user name (or UID) and optionally the user group (or GID) to use when running the image and for any `RUN`, `CMD` and `ENTRYPOINT` instructions that follow it in the `Dockerfile`.

Note that when specifying a group for the user, the user will have *only* the specified group membership. Any other configured group memberships will be ignored. When the user doesn’t have a primary group then the image (or the next instructions) will be run with the `root` group.

If a service can run without privileges, use `USER` to change to a non-root user. Start by creating the user and group in the `Dockerfile` with something like `RUN groupadd -r postgres && useradd --no-log-init -r -g postgres postgres`.

Users and groups in an image are assigned a non-deterministic UID/GID in that the “next” UID/GID is assigned regardless of image rebuilds. So, if it’s critical, you should assign an explicit UID/GID.

**Best Practices:** Avoid installing or using `sudo` as it has unpredictable TTY and signal-forwarding behavior that can cause problems. If you absolutely need functionality similar to `sudo`, such as initializing the daemon as `root` but running it as non-`root`, consider using [“gosu”](https://github.com/tianon/gosu). Lastly, to reduce layers and complexity, avoid switching `USER` back and forth frequently.

#### `VOLUME`

#### `WORKDIR`

```dockerfile
WORKDIR /path/to/workdir
```

The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the `Dockerfile`. If the `WORKDIR` doesn’t exist, it will be created even if it’s not used in any subsequent `Dockerfile` instruction.

The `WORKDIR` instruction can be used multiple times in a `Dockerfile`.

The `WORKDIR` instruction can resolve environment variables previously set using `ENV`. You can only use environment variables explicitly set in the `Dockerfile`. For example:

```dockerfile
ENV DIRPATH=/path
WORKDIR $DIRPATH/$DIRNAME
RUN pwd
```

The output of the final `pwd` command in this `Dockerfile` would be `/path/$DIRNAME`.

**Best Practices:** For clarity and reliability, you should always use absolute paths for your `WORKDIR`. Also, you should use `WORKDIR` instead of proliferating instructions like `RUN cd … && do-something`, which are hard to read, troubleshoot, and maintain.

#### `ONBUILD`

#### `RUN`

#### `CMD`

#### `ENTRYPOINT`

#### `HEALTHCHECK`

#### `SHELL`



### Practices

#### Set shell aliases or functions for Docker containers

For interactive shells, by adding it to the user's `.bashrc`:

```dockerfile
FROM foo
RUN echo 'alias hi="echo hello"' >> ~/.bashrc
```

```sh
docker build -t test .
docker run -it --rm --entrypoint /bin/bash test hi
/bin/bash: hi: No such file or directory
docker run -it --rm test bash
user@container $ hi
hello
```

For non-interactive shells you should create a small script and put it in your path, like:

```dockerfile
RUN echo -e '#!/bin/bash\necho hello' > /usr/bin/hi && \
    chmod +x /usr/bin/hi
```

If your alias uses parameters (ie. `hi Joe` -> `hello Joe`), just add `"$@"`:

```dockerfile
RUN echo -e '#!/bin/bash\necho hello "$@"' > /usr/bin/hi && \
    chmod +x /usr/bin/hi
```

```sh
docker build -t test .
docker run -it --rm --entrypoint /bin/bash test hi Joe
hello Joe
```

#### Using `gosu` vs USER

`Dockerfile`s are for creating images. Use `gosu` as part of a container initialization when you can no longer change users between run commands in your `Dockerfile`.

After the image is created, something like `gosu` allows you to drop `root` permissions at the end of your `entrypoint` inside of a container. You may initially need `root` access to do some initialization steps (fixing uid's, host mounted volume permissions, etc). Then once initialized, you run the final service without `root` privileges and as `pid` 1 to handle signals cleanly.

Refer to [Docker: using gosu vs USER](https://stackoverflow.com/questions/36781372/docker-using-gosu-vs-user#37931896) discussion on stack overflow with [jenkins docker image](https://github.com/bmitch3020/jenkins-docker) example.

```sh
  # Add call to gosu to drop from root user to jenkins user
  # when running original entrypoint
  set -- gosu jenkins "$@"
```

Check example scripts at [Yogeek's GitHub gist](https://gist.github.com/yogeek/bc8dc6dadbb72cb39efadf83920077d3).

To install `gosu` in a docker image with `Dockerfile`:

```dockerfile
RUN set -x \
  && curl -sSLo /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)" \
  && curl -sSLo /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture).asc" \
  && export GNUPGHOME="$(mktemp -d)" \
  && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
  && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
  && rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
  && chmod +x /usr/local/bin/gosu \
  && gosu nobody true
```

**Hint:** `gosu nobody true` is a smoke test to verify that the downloaded/installed binary is working appropriately. [[GitHub: tianon/gosu]](https://github.com/tianon/gosu/issues/61#issuecomment-486837963)

#### User/Group management in Alpine Linux image

For production setup, we can add a user/group with ID matching that on the production machine. In most Unix-like systems first user/group ID is 1000. Hence we can add a group & a user with GID/UID set to 1000.

```dockerfile
RUN addgroup -g 1000 lemp && adduser -u 1000 -G lemp -g lemp -s /bin/sh -D lemp
RUN chown lemp:lemp /var/www/html
```



[<Prev](#Dockerfile) [Top](#Docker-Command-Reference) [Next>](#Docker-Compose)

## Talking to the outside world

Containers live in an isolated world and changes made to them are not persistent until they are provided means to converse with the outside world. Generally, following are the means to transfer the information in and out of a container:

- Network Ports
- Bind Mounts
- Volume Mounts
- Passing environment variables at startup
- Writing container logs

### Network Ports

Port Mapping allows a container to expose a network end point by publishing a port in the Docker container to a port on the Docker host.

For example, listening port 8080 of a **Flask** *REST API* can be exposed outside the container and  mapped on port 8080 of the Docker host as

```sh
docc run -p 8080:8080 -d --rm --name api flaskapp:1.0
```

Status of the API container (running as a service) can be `inspect`ed as

```sh
docc inspect --format '{{.State.Health.Status}}' api
```

A normally running container will return a `healthy` status. Application log can be seen with this command

```sh
docc logs api
```

This will reveal AP logs of incoming requests and respective  responses.

[<Prev](#Talking-to-the-outside-world) [Top ](#Docker-Command-Reference) [Next>](#Alpine-Linux-commands)

## Docker-Compose



[<Prev](#Docker-Compose) [Top](#Docker-Command-Reference) 

## Running GUI application within a container

The main use-case for containers are servers, daemons and applications run in headless mode. They are mostly used to develop and deploy applications. However, containers can also be used to run desktop (GUI) applications in a sandbox. The key to this use-case is the X server running on the host operating system. We need to allow an X client to connect to the host X server to display its GUI on host's display. This is achieve by using `docker run` options `-e` (environment variable) and `-v` (volume). Here, we pass `$DISPLAY` variable from host shell with `-e` and mount host's X server socket from host to the container as a shared volume with `-v`. Finally, we need to allow non-network local connections to the host's X server. This is achieved by updating access control list using `xhost` command. [[Ref# 5,6](#References)]

We build a container for **Firefox browser** from an `ubuntu:latest` image. The `Dockerfile` looks like:

```dockerfile
FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive
RUN apt update \
    && apt-get -q -y install firefox \
    libcanberra-gtk-module \
    libcanberra-gtk3-module \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

CMD ["/usr/bin/firefox"]
```

To build the image, run:

```sh
docker build -t browser-box .
```

to run this container we use following script:

```sh
#!/usr/bin/env sh

xhost + local:root
docker run --rm -it --name firefox -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro browser-box
xhost - local:root
```

and viola!

![](img/docker_firefox_clip1.png)

[<Prev](#Running-GUI-application-within-a-container) [Top](#Docker-Command-Reference) 

## Alpine Linux commands

Since Alpine is based on BusyBox, its commands (e.g. `adduser` and `addgroup`) are different from Debian, Ubuntu etc.

### User management

Users & groups are frequently added to a docker container build for a variety of reasons. Options for `adduser` command are:

```sh
Usage: adduser [OPTIONS] USER [GROUP]

Create new user, or add USER to GROUP

        -h DIR          Home directory
        -g GECOS        GECOS field
        -s SHELL        Login shell
        -G GRP          Group
        -S              Create a system user
        -D              Don't assign a password
        -H              Don't create home directory
        -u UID          User id
        -k SKEL         Skeleton directory (/etc/skel)
```

For `addgroup`:

```sh
Usage: addgroup [-g GID] [-S] [USER] GROUP

Add a group or add a user to a group

	-g GID	Group id
	-S	Create a system group
```

To change password:

```sh
Read user:password from stdin and update /etc/passwd

	-e,--encrypted		Supplied passwords are in encrypted form
	-m,--md5		Encrypt using md5, not des
	-c,--crypt-method ALG	des,md5,sha256/512 (default sha512)
	-R,--root DIR		Directory to chroot into
```

An usage example of these commands in a `Dockerfile` is as follows:

```dockerfile
FROM alpine:latest

# Create a group and user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Tell docker that all future commands should run as the appuser user
USER appuser
```

another example:

```dockerfile
FROM alpine:latest

# Create a group and user
RUN addgroup \
    -S -g 1000 \
    git && \
  adduser \
    -S -H -D \
    -h /data/git \
    -s /bin/bash \
    -u 1000 \
    -G git \
    git && \
  echo "git:$(dd if=/dev/urandom bs=24 count=1 status=none | base64)" | chpasswd
```



[<Prev](#Alpine-Linux-commands) [Top](#Docker-Command-Reference)



## References

1. Docker docs
2. Dockerfile reference
3. Dockerfile best practices
4. Docker compose reference
5. Lei Mao's [log book](https://leimao.github.io/blog/Docker-Container-GUI-Display/).
6. [linuxmeerkat](https://linuxmeerkat.wordpress.com/2014/10/17/running-a-gui-application-in-a-docker-container/) blog.