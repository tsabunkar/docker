# Assignment : CLI App

- Use diffenret Linux distro (Distribution) containers to check curl cli tool version
- Use two different terminal to start bash in both centos:7 and ubuntu:14.04, using -it
- Learn about: 'docker container --rm'
- Ensure curl is installed and on latest version for that distro
  - Ubuntu: apt-get update && apt-get install curl
  - Centos: yum update curl
- Check the 'curl --version'

---

# My-Solution

- \$ docker pull ubuntu:14.04
- \$ docker image ls -a
- \$ docker run -d --name ubuntu_os ubuntu:14.04 (WRONG Cmd, plz run OS in interactive mode as below)
- \$ docker container run -it ubuntu:14.04 bash
- root@eb5384e448e1: \\$ apt-get update && apt-get install curl (Inside interactive mode of ubuntu 14.04)
  root@eb5384e448e1: \$ curl --version ==> (o/p -> curl 7.35.0 )
- \$ docker pull centos:7
- \$ docker image ls -a
- \$ docker container run -it centos:7 bash
- [root@890d7f51f476 /]# curl --version ==> (o/p -> curl 7.29.0 )
- \$ docker container ls -a
- docker container rm <ContainerID> (Need to manually remove the container from containers list)
  ex - docker container rm 2cd0e4e18b28

---

# Actual Solution:

- \$ docker container run --rm -it centos:7 bash (--rm cleanly removes the container when exit, thus no need to remove this exited container manually).
- \$ curl --version

- \$ docker container ls -a
- \$ docker container run --rm -it ubuntu:14.04 bash
- \$ apt-get update && apt-get install curl
- \$ curl --version
