# Container Images

- All about images, the building blocks of containers
- What's in an image ( and what isn't)
- Using Docker Hub registry
- Managing our local image cache
- Building our own images :D

---

# What's in an image (and what isn't)

- Image are -> App binaries and dependencies
- Image are NOT -> Complete OS, NOT Kernel, NOT Kernel Modules(ex- drivers)
- Thus Images are just binaries that you application(mysql) need, bcoz the host provides the Kernel. THis is the biggest feature that container provides over typical VM's (Bcoz containers are not booting up full Operation System, but it really just starting the application)
- Image has metadata about the image data and how to run the image
- Official definitation : "An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime"
- Images can be small as one file (you app binary) like a golang static binary
- Image can be as big as Ubuntu distro with apt, Apache, PHP and more installed

---

# Using Docker Hub registry

- Docker hub, "The apt package system for Containers"
- Docker hub have different base image like Debian or Alpine, For ex-
  - We have Debian Image for nginx as : 1.17.4, mainline, 1, 1.17, latest
  - Alpine Distro image for nginx as : 1.17.4-alpine, mainline-alpine, 1-alpine, 1.17-alpine, alpine
- How to identify official image repositry ? ==>
  - It will be written official
  - It will not have forward slash in the repo name for ex- nginx <-- is the official repo, where as bitnami/nginx is ducplicate (where - bitnami account/orginzation name)
- Docker image has 'tag' [ which are like versions ] ex- latest (does not means latest commit, but it means currently latest commit you have pulled on that time)
- Note in production, we must specifiy the exact version or tag of the image we want rather than specific more generic like latest
- Docker file official images list : https://github.com/docker-library/official-images/tree/master/library

---

# Image Layers :

- Images are designed by union file system concept
- \$ docker image ls
- \$ docker history nginx:latest ==> Docker image history :- Shows the layers of changes made in that image, This is not the list that had happen in the container but rather it is history about the image layer
- Every image start from - blank layer called ==> Sratch
- Then after that, Every set of changes that happens on the file system in the image is a layer ( It can be one layer or more layer)
- Thus this cmd shows the list of layer changes that happend for that particualr image
- Every layer has its own unique SHA
- We think our own image as :
  - Add Ubuntu distro image
  - On that ubuntu, Add apt package manager
  - On above that add, Environment variable
  - Thus all these can be termed as one single image
    ENV
    APT
    UBUNUTU
  - Note : Each of these can be called as layer
- Caching - Simply means if the image is not avaliable locally, then pull it from Docker Hub Repositry
- Caching of layer -
  - Means suppose we have Ubuntu:17 Distro layer, now we have added apt package manager ==> Consider this as our own image
  - Now you have created one more Ubuntu:17 Distro layer, but u want yum as package manager ==> Consider this is our second image
  - But while in second image - Docker will use the cached layer of Ubuntu:17 locally and only download the yum package manager
  - Vizualization : container-images/image-layer.png

---

# Container Layer

- Vizualization : container-images/container-layer.png
- Consider we have apache image, we want container top of it, Docker we will create a new read & write layer for that container, on top of Apache image
- Similarly we have container-2 and container-3 instance
- These containers are layers as stack
- When we are running containers and changing a file on the image, The file system will take that file differenceing file and copy it specific container (i.e-container) ==> COW (Copy on Write)
- \$ docker history mysql:8.0.18
- \$ docker image inspect mysql:8.0.18 ==> Returns JSON metadata about the image
  ( We know that image is made up of binaries, its dependencies and meta-data about that image :: Thus inspect of image gives the meta-data about that image )
  ( We get information about the 'ExposedPorts', 'Env', etc)

# Conclusion of Images and Their Layers:

- Images are made up of file system changes and metadata
- Each layer is uniquely identified by SHA and only stored once on a host
- This layer concept saves storage space on host and transfer time on push/pull
- A container is just a single read/write layer on top of image
- docker image history and docker image inspect commands tell us about the history of the image and gives the meta-data about that image

---
