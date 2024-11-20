### Dockerfile

- [[Docker 101]](https://dockerlabs.collabnix.com/workshop/docker/)
- [[Dockerfile reference]](https://docs.docker.com/reference/dockerfile/)

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

### docker attach

```sh
docker attach [OPTIONS] CONTAINER # attach to a detached container
              --detach-keys=KEYS  # assign a detach key like ctrl-b
```

To detach from a running container: press detach keys, default is `ctrl-p`+`ctrl-q`.

### docker exec

```sh
docker exec [OPTIONS] CONTAINER CMD # run the command in a running container
            -t                      # run in tty then exit
            -it                     # run in an interactive tty
```

### docker ps

```sh
docker ps [OPTIONS] # list all running containers
          -a        # list all recorded containers (running/paused/exited)
          -q        # quiet mode, container_id only
```

### docker inspect

```sh
docker inspect DOCKER_OBJECT # print object info in json format
                             #  - image
                             #  - container
                             #  - volume
                             #  - network
```

### docker image

```sh
docker build ...                           # build a new image
docker images [FILTER]                     # list all images, alias of docker image ls
docker pull IMAGE[:TAG]                    # pull an image from docker hub
docker tag IMAGE[:TAG] NEW_IMAGE[:NEW_TAG] # rename an image
docker image rm [-f] IMAGE                 # remove an image
docker image prune -a                      # remove all unused images
docker history IMAGE[:TAG]                 # print how the image was built
```

### docker container

`CONTAINER` can be container_id (short-form OK) or container_name.

```sh
docker run ...                  # new a container
docker ps ...                   # list container status
docker stop CONTAINER           # as it says
docker kill CONTAINER           # as it says
docker start/restart CONTAINER  # as it says
docker pause/unpause CONTAINER  # as it says
docker logs [OPTIONS] CONTAINER # print stdout of container
            -f                  # follow mode
            -n N                # only tail N lines
docker port CONTAINER [PORT]    # show port mapping info
docker rm [-f] CONTAINER        # remove record from docker ps
docker rm $(docker ps -aq)      # remove all records from docker ps
```

### docker volume

```sh
docker volume create VOLUME             # create a volume
docker volume ls                        # list all volumes
docker volume rm [-f] VOLUME            # remove a volume
docker volume rm $(docker volume ls -q) # remove all volumes
```

### docker network

Network has multiple types (a.k.a. drivers) [[Reference]](https://docs.docker.com/engine/network/#drivers) :

- **bridge**: communicate among containers only, publish specific ports to docker host, the default type.
- **host**: directly expose on host machine, publish options are ignored.
- **none**: no network provided.

```sh
docker network create -d DRIVER NETWORK     # create a network of specific type
docker network ls                           # list all networks
docker network connect NETWORK CONTAINER    # connect a network to container
docker network disconnect NETWORK CONTAINER # disconnect a network to container
docker network rm NETWORK                   # remove a network
```

### docker compose

- [[Docker compose 101]](https://dockerlabs.collabnix.com/intermediate/workshop/)
- [[compose.yaml reference]](https://docs.docker.com/reference/compose-file/)

- A `compose.yaml` file is wanted in working directory.
- `service` in compose is equivalent to `container`.

```sh
docker compose config                       # print info of in-place compose.yaml
docker compose build                        # build images defined in compose.yaml
docker compose up [OPTIONS]                 # start current compose
                  -d                        # detached mode
                  --build                   # rebuild images (if needed) then restart
                  --no-start                # create services only but don't start
docker compose down [OPTIONS]               # the BEST way to stop compose
                    --volumes               # also remove created volumes
                    --rmi local             # also remove local built images by current compose
docker compose images                       # list images used by current compose
docker compose ps [-a] [-q] [--services]    # list services used by current compose
docker compose start/stop/restart [SERVICE] # as it says
docker compose pause/unpause [SERVICE]      # as it says
docker compose kill [SERVICE]               # as it says
docker compose logs [-f] [-n N] [SERVICE]   # print/follow stdout in service
docker compose run SERVICE CMD              # run a one-time command in a new service, not the running one
docker compose rm [OPTIONS] [SERVICE]       # remove stopped service from ps
                  -s                        # stop and remove
```