### docker build

```sh
docker build -t IMAGE[:TAG] . # build image from in-place Dockerfile
```

### docker run

```sh
docker run [OPTIONS] IMAGE[:TAG] [CMD] # run a container from image (and run the command)
           -d                          # detached mode
           -it                         # invoke an interactive tty
           -P                          # publish all EXPOSE ports
           -p HOST_PORT:CONTAINER_PORT # map a container port to docker host
           -v VOLUME:CONTAINER_PATH    # mount volume on container path
           --network NETWORK           # connect to a network
           --name CONTAINER            # name the container
           --rm                        # one-off mode, auto remove from docker ps after exited
```

### docker exec

`CONTAINER` can be container-hash (short-form OK) or container-name.

```sh
docker exec [OPTIONS] CONTAINER CMD # run the command in a running container
            -it                     # run in an interactive tty
```

### docker ps

```sh
docker ps [OPTIONS] # list all running containers
          -a        # list all recorded containers (running/paused/exited)
          -q        # quiet mode, container-id only
```

### docker inspect

```sh
docker inspect DOCKER_OBJECT # print object info in json format
                             #  - image
                             #  - container
                             #  - volume
                             #  - network
```

### Attaching and detaching container

```sh
docker attach [OPTIONS] CONTAINER # attach to a detached container
              --detach-keys=KEYS  # assign a detach key like ctrl-b
```

To detach from a running container: press detach keys, default is `ctrl-p`+`ctrl-q`.

### Managing images

```sh
docker build ...                           # build a new image
docker images [FILTER]                     # list all images, alias of docker image ls
docker tag IMAGE[:TAG] NEW_IMAGE[:NEW_TAG] # rename an image
docker image rm [-f] IMAGE                 # remove an image
docker image prune -a                      # remove all unused images
docker history IMAGE[:TAG]                 # print how the image was built
```

### Managing containers

```sh
docker run ...                   # new a container
docker ps ...                    # list container status
docker stop CONTAINER            # SIGINT to container
docker kill CONTAINER            # SIGKILL to container
docker start/restart CONTAINER   # as it says
docker pause/unpause CONTAINER   # as it says
docker logs [--follow] CONTAINER # print/follow stdout in container
docker rm [-f] CONTAINER         # remove record from docker ps
docker rm $(docker ps -aq)       # remove all records from docker ps
```

### Managing volumes

```sh
docker volume create VOLUME             # create a volume
docker volume ls                        # list all volumes
docker volume rm [-f] VOLUME            # remove a volume
docker volume rm $(docker volume ls -q) # remove all volumes
```

### Managing networks

Network has multiple types (a.k.a. drivers) [[Reference]](https://docs.docker.com/engine/network/#drivers) :

- **bridge**: communicate among containers only, publish specific ports, default type.
- **host**: directly expose on host machine, publish options are ignored.

```sh
docker network create -d DRIVER NETWORK     # create a network of specific type
docker network ls                           # list all networks
docker network connect NETWORK CONTAINER    # connect a network to container
docker network disconnect NETWORK CONTAINER # disconnect a network to container
docker network rm NETWORK                   # remove a network
```