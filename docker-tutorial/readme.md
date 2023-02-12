
- [Introduction](#introduction)
- [Commands](#commands)
- [Docker Network](#docker-network)
- [Dockerfile](#dockerfile)
- [Volumes](#volumes)
- [Notes](#notes)


# Introduction

**Docker:** Set of PaaS products that use OS-level virtualization to deliver software in packages called containers. The service has both free and premium tiers. The software that hosts the containers is called Docker Engine

**Container:** A way to pacakge applications with all the necessary dependencies and configs. It is a running environment for an Image.

Docker images can be hosted to registries, the naming convention is `registryDomain/imageName:tag`

`docker pull mongo:4.2` is basically a short form of `docker pull docker.io/library/mongo:4.2`

`OS = Hardware --> Kernel layer --> Application layer`

| Docker                                                               | Virtual Machine                               |
| -------------------------------------------------------------------- | --------------------------------------------- |
| Virtualizes application layer                                        | Virtualizes both Kernal and Application layer |
| Images are small                                                     | Images are large                              |
| Easier and faster to start/stop                                      | Takes more time to start/stop                 |
| Less resource hungry                                                 | More resource hungry                          |
| Docker is linux based so on non-lunix OS you will need Docker engine | VM of any OS can run on any OS Host           |

# Commands

*`containerId` can be replaced by `containerName`*

`docker pull imageName` // download image

`docker run imageName:version` // run image

`docker run -d imageName` // run image in detach mode

`docker run --name customName imageName` // run image, "docker ps" will use "customName" for this container's name insted of the random name

`docker run -p hostPort:containerPort imageName` // run the image and bind the container port to the host port

`docker run -p 27017:27017 -d -e MONGO_INITDB_ROOT_USERNAME=rizwan -e MONGO_INITDB_ROOT_PASSWORD=minhas --name mongodb --net=networkName mongo` // `-e` is used for environment variables.

`docker ps` // list running containers

`docker ps -a` // show all containers, running or stopped.

`docker ps -a | grep my-app` // show all stopped or live containers with my-app name

`docker rm containerId` // delete a container

`docker images` // list images

`docker rmi imageId` // delete an image

`docker stop containerId` // stop the container

`docker start containerId` // start the container

`docker logs containerId` // shows the container logs

`docker exec -it containerId /bin/bash` // get the terminal of the running container. `-it` means interactive mode, `/bin/bash` will be executed. You can also use `/bin/sh` in case if the container doesn't have bash installed

`docker network ls` // list all the docker network

`docker network create networkName` // create a new docker network

`docker build -t my-app:1.0 .` // Builds an image from a `Dockerfile`. `-t` is used to tag the image. `.` means read the `Dockerfile` from the current location. Running `docker images` will show `my-app` under `REPOSITORY` and `1.0` under `TAG`. `docker run my-app:1.0` will run the container.

`docker tag` # used to rename an image

# Docker Network

Docker creates an isolated container network where the containers can talk to each other by their names without the need of localhost, portnumbers, etc. The host applications need to connect the docker containers using `localhost` and the port that was binded to the container.

# Dockerfile

*A blueprint for a docker image just like a Class is a blue print for an object in OOP.*

```
FROM baseImage # define the base image of this container

ENV MY_VAR_NAME=test # define env variables in the container

RUN mkdir -p /home/app # RUN executes any linux command in the container

ENTRYPOINT ["shellscript.sh"] # define a shell script that should run on the container as an entrypoint

COPY . /home/app # COPY executes on the host

CMD ["command 1", "command 2", "command n"] # executes these commands in the container or executes entrypoint linux commands. All commands are concatenated with a space and executed together.
```

`RUN vs CMD` You can have multiple `RUN` but only one `CMD` because `CMD` is an entrypoint command.

# Volumes
Docker volumes are used for persistence, we plug the host file system to the virtual file system of the container.

There are 3 main ways to create volumes:

1. `docker run -v /host/path:/container/path`
2. `docker run -v /var/lib/mysql/data` docker will automatically create a volume on host at `/var/lib/docker/volumes/random-hash/_data`. This is known as `anonymous volume`
3. `docker run -v myName:/var/lib/mysql/data` // docker will create `/var/lib/docker/volumes/myName/_data`. This is known as `named volume`. This is recommended way to create volumes.

# Notes
- When all containers are running in the docker-compose then you should use container name instead of localhost, you don't even have to mention the port name because the port names are already present in the docker-compose.