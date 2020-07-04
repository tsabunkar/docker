# Docker Basic Commands

- \$ docker pull centos:centos8
- \$ docker run centos:centos8
- \$ docker run -it centos:centos8 bash (run bash command inside the running container)
- \$ # cat /etc/redhat-release
- \$ # exit
- \$ docker run -d centos:centos8 sleep 10 (run in background in Dettached mode)
- \$ docker container ls
- \$ docker container ls -a
- \$ docker run -d centos:centos8 sleep 2000
- \$ docker container ls
- \$ docker stop 10ca53b41ff1 (stopping a container)
- \$ docker container ls -a [check exit code for previously stopped/killed container -> Exited (137)]
- \$ docker rm 10ca53b41ff1
- \$ docker image ls
- \$ docker image prune (Removed unused images)
- \$ docker image rm c6d035ac9b1c (Remove a particular image)
- \$ docker image rm -f c6d035ac9b1c (force remove an image)
- \$ docker run -d centos:centos8 sleep 2000
- \$ docker container ls
- \$ docker container exec 02de44101cdd cat /etc/redhat-release (excutes command inside a running container)

---

# Quiz

- https://katacoda.com/kodekloud/scenarios/docker-for-beginners-fcc-basiccommands

- \$ docker version (version of Docker Server Engine running on the Host)
- \$ docker container ls | wc -l (count containers are running on this host - 1 )
- \$ docker image ls | wc -l (count image downloaded on this host - 1 )
- \$ docker run redis (run redis image instance as container)
- \$ docker container ls -a | wc -l( containers are PRESENT on the host -> Including both Running and Not Running)
- \$ docker container ls -a [States is - EXIT ] ==> (state of the stopped alpine container)
- \$ docker container ls -aq (To list all the running container --> Only display container ID )
- \$ docker container stop \$(docker container ls -aq) [To Stop all containers running]
- \$ docker container rm \$(docker container ls -a -aq) [To Remove all containers from disk]
- \$ docker image rm ubuntu (Delete the ubuntu Image)
- \$ docker run -d --name webapp nginx:1.14-alpine (Run a container with the nginx:1.14-alpine image and name it webapp)
- \$ docker rm -f $(docker ps -a -q) && docker rmi -f $(docker images -q) [To remove all running containers ever and all images pulled ever]

---
