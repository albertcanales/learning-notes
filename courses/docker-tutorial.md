[Docker Tutorial for Beginners - Techworld with Nana](https://youtu.be/3c-iBn73dDE)

## What is Docker?

> This section was already familar for me from various introductory videos on the topic.

## What is a Container?

A container is made of layers of images. 

Dividing a container into layers makes pulling diferent versions of the same container much faster, for example.

The difference between image and container is similar to the one between program and process.

## Docker vs Virtual Machine

A hosts consists of three layers:

2. Applications
1. Kernel
0. Hardware

While VMs virtualise layers 1 and 2, Docker only virtualises 2. This results in better size, speed, but sacrifices compatibility.

## Basic commands

`docker run` options:

- `-d`: detached mode
- `-p<host_port>:<container_port>`: Binds the given host and container port
- `--name <name>`: Assigns a certain name to a container

## Debugging a Container

Docker commands for debugging:

- `logs`: See logs of a container
- `exec`: Run a command on a container. `docker exec -it <container> <shell>` gives an access to the shell

## Demo Project Overview

Interesting example of CI, from development to deployment with various technologies. 

## Developing with Containers

> I am not really interested the web technologies used in this example, as I am looking for a sysadmin usecase for docker, so a lot of content won't be written about.

### Docker Networks

By grouping containers with a network, data can be transfered between only with their names. 

The `network` command is used to create, list... networks. A container can belong to a network by specifying `--net` on the `run` command.

## Docker Compose

Uses YAML to structure the run commands on a file. Docs [here](https://docs.docker.com/compose/). Important features:

- Automatically creates a common network for all the services specified in a file.

Usage: 

- Start containers (and crates network): `docker-compose -f <file> up`.
- Stops containers (and removes network): `docker-compose -f <file> down`.

## Dockerfile

A blueprint to create docker images. Very strict and simple syntax, documentation [here](https://docs.docker.com/engine/reference/builder/). Example:

```
FROM <base_image>

ENV <envvar_name>=<env_var_value> \
    ...

RUN <shell_command>
...

COPY <host_path> <container_path>
...

CMD <entrypoint_command>
```

Images are based on more basic images through Dockerfiles. This is called *image layers*

An image is created from a Dockerfile with the `build` command. `-t` specifies the tag.

## Private Docker Repository

> This section is also not very interesting for my intended use of Docker for now, so I am skipping it.

## Docker Volumes

Docker Volumes compensate the lack of persistency of the Virtual File System of the Container. A Volume is nothing more than a host file (normally a directory) mounted on the Virtual File System.

There are three type of volumes:
- Host volume: Host and container paths are specified.
- Anonymous volume: Container path is specified but the host one is automatically generated.
- **Named volume**: Same as anonymous but a name is given to the volume.

Volumes can be defined also in Docker Compose.

Anonymous and named volumes are stored in `/var/lib/docker/volumes`.


