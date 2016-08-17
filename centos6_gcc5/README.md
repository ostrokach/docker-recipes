# CentOS 6 / GCC 5 Docker Image


## Build container

```bash
$ docker build .
```

This will take a while...


## Tag container

```bash
# Find the right image
$ docker images

# Tag the image with an appropriate name
$ docker tag {image_id} ostrokach/centos6_gcc5:latest
```


## Squash container

```bash
# Start container so that we can export it
$ docker start centos6_gcc5:latest

# Find the `CONTAINER ID` of the running container
$ docker ps

# Export the running container, and import it back again
# Source: http://tuhrig.de/flatten-a-docker-container-or-image
$ docker export {container_id} | docker import - ostrokach/centos6_gcc5:squashed
```


## Push to dockerhub

```bash
# Login using dockerhub credentials.
$ docker login

# Push image
$ docker push ostrokach/centos6_gcc5:squashed
```

