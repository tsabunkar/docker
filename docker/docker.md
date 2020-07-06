# Why do we need docker ?

- Suppose web have webserver- nodejs, db- mongodb, messageing- Redis, Orchestration- Ansible, different version of this lb might mis-match with different version of dependencies , different OS, Hardware Infrastructure, etc which will lead to --> "THE MATRIX FROM HELL"
- [.assets/matrix-hell.png]
- If a new developer joins the team, we need to setup his environment (Large set of instructions to be followed- Manual Effort and set-up Time is wasted) [Make sure right version of dependencies, OS, etc]
- Long setup time
- Compatibility / Dependency
- Different Dev/Test/Prod envrionments

---

# Usage of Docker

- With Docker we can run each component in its separate container (environment) with its own libs/Dependency
- [./assets/docker.png]
- Thus with docker installed in new developer machine (irrespective of OS)- Then need to just run the docker file, NO need to do step of installation of s/w, libraries and set-up his system manually
- What can docker do :
  - Containerize Applications
  - Run each service with its own dependencies in separate containers

---

# Containers

- Containers are completely isolated environemnts
- Containers can have there own -> process, services, network interfaces, mounts (like VM except the all share the same OS Kernel)
- different containers are lxc, lxd, etc (docker uses lxc)
- [.assets/containers.png]

---

# Operating System

- Operating System consist of two things - Softwares and OS Kernel
- OS Kernel interact with Computer Hardware
- OS Kernel --> LINUX, Above layer Software --> Ubuntu, Fedora, Suse, CentOS
- thus, Docker container share the underlying Kernel
- [.assets/os.png]

---

# Sharing the Kernel

- Let us consider, OS - Ubunutu (Host Machine)
- Docker is installed in Ubuntu Host OS
- We can installed any layer of software (i.e- fedora, CentOs, Suse, etc) as there base OS Kernel --> LINUX based (Ubunutu)
  - i.e- we can run Suse, fedora, etc container on host machine as Ubunut (since base OS Kernel is Linux)
- [.assets/sharing-kernel.png]
- NOTE : We cannot run windows container which has base OS kernel as - Ubunutu (LINUX) [Even vice-versa is true]
  - This is dis-advantage right, not able to run another OS in docker ? Unlike - Hypervisor, Docker is not ment to virtualize --> different os & different os kernel and run on same hardware
  - Docker is ment to - package, containerized application, ship them and run them anywhere and anytime

---

# Difference Containers v/s Virtual Machines

- VM:
  - Each VM has its own Operating system, thus host machine - CPU utilization is high and Space(disk space) Allocated is high and fixed (in GB)
  - since need to boot-up entier system -> high booting time
  - vm has complete isolation with each other - no sharing of resources b/w VM
- Containers:
  - Uses less disk space (in MB)
  - Very less CPU utilization
  - Containers are light weight and boot faster
  - less isolation and more resources are shared between the containers
- [.assets/container-VM.png]

---

# Containers & VM

- Current system we can take advantages of both world- Containers and VM
- [.assets/containers&VM.png]

---

# Docker Hub

- Most of the companies have their- Containerized Application readily avaliable in dockerhub [i.e- Oracle has containerized application for java which you can pull from docker hub using --> docker pull store/oracle/serverjre:1.8.0_241-b07]
- https://hub.docker.com/
- Docker hub is public/private repos where we can find images of os (ubuntu, CentOS), applications (nodejs, pythong), etc [which is been containerized by companies/manufacturer from that s/w creator]
- Now if we want to run that install the Redis application locally ---> we pull the image of Redis from docker hub and run instances of redis application in our docker host i.e-
  - \$ docker pull redis (Pull from docker hub registry in to docker host locally)
  - \$ docker run redis (run instance(contianer) of Redis)
- We can run as many instance (containers) of docker image which we pulled (downloaded) from docker Hub Registry
- We can also create our own docker image and push it to Docker hub registry and make it public for other to use it

---

# Container v/s Image

- Image:
  - Think Docker Image as package or template or plan
  - Image think as ISO Image (from VM world) which you downloaded for kali linux, where as using Oracle VirtualBox you can run many instance of kali linux using this ISO image --> think each running instance of VM as containers
- Containers:
  - Running instances of image
  - Each container have their own environment, network, mounts, etc
  - containers are isolated form each other but can share same resource as run os same OS kernel

---

# Docker in DevOps

- We had 2 Teams Developers and Operations
- Developer - develop the application and handover to Ops (Operation) team, Operation - Manage this application in Dev/Prod environments
- Traditionally developer use to provide manual steps Guide - host to setup, App.war file to deploy, dependencies to be configured etc to Ops Team in order to deploy application which result into "NOT WORK IN MACHINE" (Also Ops team did not develop the application- They struggled with setting it up)
- Now, This manual steps Guide is transformed into Docker file, This docker file is used to create an image. So that Developer provide this docker file to Ops Team. Thus this image will run on any docker host and machine, thus ops team can directly deploy this instance of docker file iamge provided by developer.
- Therefore Docker contributes for DevOps Culture

---

# Docker Editions

- Community Edition (FOSS)
- Enterprise Edition (Paid version)

---

# Commands

- Download an image locally:
  - \$ docker pull docker/whalesay
- Run(start) a container:
  - \$ docker run docker/whalesay cowsay boo
  - (if not present locally it pull from Docker hub Registry)
  - (run the instance of nginx application if the image is already exist locally)
- List Containers
  - \$ docker container ls
  - \$ docker container ls -a (containers that are stopped)
- Stop Container
  - \$ docker container stop <container_id>
- Remove container
  - \$ docker container rm <container_id>
- List of Images
  - \$ docker images ls
  - \$ docker images ls -a (image that is stopped)
- Remove image
  - \$ docker image rm <image_id> (before stop and remove all the previous containers ran)
- Running docker container of OS
  - \$ docker pull alpine:3.12.0
  - \$ docker run alpine:3.12.0
  - (When we run the instance of alpine OS, docker run this container and exit immediately, If we list the running containers we cannot see the container running Reason ---> Unlike VM - containers are not ment to host an operating system, whereas containers are ment to run a specific task i.e- run webserver, run application server, run db instance, etc)
  - Container lives only uptill the process inside it is alive for ex- if web server inside the container is stopped/crached the container exist. but when we run the instance of alpine os it stop immediately as generally os container is used as base image for other applications
  - If docker run command is not running with specific process like - \$ docker run alpine:3.12.0
    (then we can run this image with specific process like - \$ docker run alpine:3.12.0 sleep 5)
- Exec - Execute a command inside Running container
  - (if we want to run command inside the running container - [if that container application provide bash environ] then we can use "exec" command)
  - \$ docker container ls -a
  - \$ docker exec <container_id/container_name> cat /etc/hosts (print the file hosts present inside etc directory)
- Run - Attach and detach
  - \$ docker run kodekloud/simpe-webapp [Attached Mode]
    - (It run in foreground in attach mode - we are attached to CLI of this running container application and we can interact with this running container application instance)
  - \$ docker run -d kodekloud/simpe-webapp [Deattached Mode]
    - (Will run this container in deattached mode, it will run the docker container in background mode)
  - \$ docker container attach <container_id> (Dynamically attach to the running container instance in Attach Mode)

---

# Docker Registry

- Cloud Repositry where docker images are stored
- Central Repositry of all docker images
- \$ docker run nginx (Runs the instances of nginx image)
  - Actually above image name is --> image: docker.io/nginx/nginx
  - First part of image name --> Central Registry Name (by default dockerHub) [other - gcr.io]
  - Second part of image name --> User/Account name (docker hub User Account name / Organization name)
  - Third part of image name --> Image/Repository name
  - If we don't give Second part of image name then docker assumes that ==> Second part of image name <--same_as--> Third part of image name
  - [.assets/docker-image-mean.png]
- If we have Organization specific Registry then (Not DockerHub Registry)
  - \$ docker login private-registry.io
  - \$ docker run private-registry.io/tsabunkar/my-web-app
  - \$ docker push private-registry.io/tsabunkar/my-web-app
  - \$ docker pull private-registry.io/tsabunkar/my-web-app
- Deploy Private Registry (within localhost of Orgnization without Cloud Solution-> inhouse Private Registry)
  - \$ docker run -d -p 5000:5000 --name registry registry:2
  - \$ docker image tag my-image localhost:5000/my-image
  - \$ docker push localhost:5000/my-image
  - \$ docker pull localhost:5000/my-image (or) docker pull 192.168.56.100:5000/my-image

---

# Container Orchestration

- With docker we can run the single instance of the application using docker run command (one instance os app on one docker Host) but what if number of client/traffic increases and this instance is not able to handle the load ? ==> We need to manually add/remove multiple instances as load increase/decrase based on traffic/usage, Closely monitor docker Host, need to check health of docker Host, container -> Thus this approach is not feasible. ==> Sol is Container Orchestration
- Container Orchestration-
  - it contains set of tools and scripts that can help host, containers in PROD environment.
  - It containes multiple docker Host which can host containers (or instance of an image) so that if one fails, we can use other docker host
  - It spun-up/deploy multiple containers in an docker host just by single command in docker swarm: \$ docker service create --replicas=100 nodejs
  - Orchestration Solution also provide scale up or down as traffic increase or decrease
  - Container Orchestration also provides advance networking b/w containers of other docker host
  - Other feature- loading balancing, sharing storage b/w docker host, configuration Management, Security, etc
- Different Container Orchestration providers are :
  - Docker Swarm (from docker)
  - Kubernetes (from google) <--- Most Popular (avaliable in all cloud providers GCP, AWS, Azure)
  - MESOS (from Apache)
- Docker Swarm
  - Single Docker Swarm can have muliple Docker Hosts (where each docker host can have muliple containers of an image) [.assets/docker-swarm.png]
  - One Docker Host is allocated as -> Swarm Manager, Other all docker hosts are -> Workers
  - \$ docker swarm init (Run in Swarm Manager)
  - \$ docker swarm join --token <token> (Run in workers)(Join Workers with Swarm Manager)
  - Once worker are joined with Swarm manager -> this workers are now called Node and now ready to create service and deploy in Swarm Cluster [.assets/docker-connect-nodes.png]
  - Docker Swarm Orchestration - provides load balancing, scale-up/down, etc.
  - key component in Docker Swarm Orchestration is -> Docker Service
  - Docker Service: Is one or more instance of an application that are running across all the nodes/workers in Docker Swarm Cluster -> \$ docker service create --replicas=3 my-web-server (run in Swarm Manager) (here we specifiy no of replicas that we want to run across worker node) [.assets/docker-service.png]
- Kubernetes:
  - Kubernetes CLI - kube control tool (kubectl)
  - Using kubectl we can run thousand instance of same application with single command.
    - \$ kubectl run --replicas=1000 my-web-server
  - We can confifure Kubernetes such that it can automatically handle traffic load and scale-up or down the instance depending on traffic, do load balancing, etc
    - \$ kubectl scale --replicas=2000 my-web-server
  - with rolling-update command it can also upgrade these thousands of instances.
    - \$ kubectl rolling-update my-web-server --image=web-server:2
    - \$ kubectl rolling-update my-web-server --rollback (Rollback updates)
  - Kubernetes provides different advance network providers, storage, security, Authentication & Authorization [.assets/k8.png]
- Relationship b/w docker and Kubernetes:
  - k8 uses Docker Host inorder to host docker containers.
  - K8 also provides alternative of docker -> Rocket
- k8 Architecture
  - consist of master and worker nodes (each node think as docker Host)
  - master manages all other worker node and responsible for actual orchestration of worker nodes [.assets/master-slave-nodes.png]
  - K8 comes with following components:
    - API Server (Act as frontend for Kubernetes Cluster )
    - etcd (distributed key value storage)
    - kubelet (Agent)
    - Container Runtime
    - Controller (think- brain of k8)
    - Scheduler (distributing the work/containers across the nodes)
