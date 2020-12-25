# Docker Command Reference 

[TOC]

[Top](#Docker-Command-Reference) [Next>](#Dockerfile)

## Docker Basics



### Pull from Docker Hub

```sh
docker image pull alpine:latest
```

### Contents of a Docker image

```sh
docker image inspect alpine | less
```

There is a lot of information, so it can be confusing, but as an example look for the values for `Cmd` and `Env` metadata. To check the required tags in the JSON output use:

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

If we don’t need to access to the container after it’s stopped we can pass the `--rm` option to `docc run ...`. For example

```sh
docc run -d --rm --name myAlpine alpine

# you can specify a custom coomand/program to run in the container
# and --rm option if you don't want to keep the container.
docc run --rm -it --name myAlpine alpine:latest /bin/sh
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

[<Prev](#Talking-to-the-outside-world) [Top](#Docker-Command-Reference)

## Docker-Compose



[<Prev](#Docker-Compose) [Top](#Docker-Command-Reference)



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



An usage example of these commands in a Dockerfile is as follows:

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