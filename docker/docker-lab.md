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
- $ docker container stop $(docker container ls -aq) [To Stop all containers running]
- $ docker container rm $(docker container ls -a -aq) [To Remove all containers from disk]
- \$ docker image rm ubuntu (Delete the ubuntu Image)
- \$ docker run -d --name webapp nginx:1.14-alpine (Run a container with the nginx:1.14-alpine image and name it webapp)
- \$ docker rm -f $(docker ps -a -q) && docker rmi -f $(docker images -q) [To remove all running containers ever and all images pulled ever]

---

# Docker Run

- \$ docker run centos:centos8 cat /etc/redhat-release
- \$ docker run centos:centos7 cat /etc/redhat-release (running different docker container instance, different images with tags)
- \$ docker run centos:centos8 sleep 1500 (CTRL+C won't work to terminate)
  - (To stop above container)
  - (open new terminal)
  - \$ docker container ls
  - \$ docker container stop <container_id>
- \$ docker run -d centos:centos8 sleep 1500 (running a server which runs permnanetly until terminated)
- \$ docker container ls
- \$ docker container attach 7425ab880483 (Attach mode to running container )
- \$ docker run timer (keeps printing time infinitely ==> Runs in Attached Mode - Foreground)
- \$ docker run -d timer
- \$ docker container attach 7425ab880483
- \$ docker pull jenkins:2.60.3
- \$ docker run jenkins:2.60.3
- \$ docker inspect cfdeb24ff21e (To find internal ipAddr of Jenkins container --> check under "Networks" - "IPAddress": "172.17.0.2",)
- (Visit: http://172.17.0.2:8080) ==> Access this docker container instance of Jenkins Running on Application internal ipAddr
- \$ ifconfig docker0 (To Know Docker HOST IpAddress)
  - NOTE: Docker Host Ip Addr is not same as Localhost (Host Machine IpAddr)
- (Visit http://172.17.0.1:9090/) ==> Docker host IpAddress is not same as the docker container of Jenkins internal IpAddress
- ( In order to make: Docker host IpAddres <--Map--> Jenkins internal IpAddress )
  - Stop jekins container instance if running
  - \$ docker run -p 9090:8080 jenkins:2.60.3
  - ( Docker Host PORT = 9090 <--Map--> Jenkins Internal Container PORT = 8080 )
  - Visit Jenkins internal ip address (container internal ip addr) ==> http://172.17.0.2:8080/
  - Visit Docker Host Ip Address ==> http://172.17.0.1:9090/
- (To Persist docker container data-> map volume)
  - \$ mkdir ~/tejas/docker-data (pwd--> /home/tejas/tejas/docker-data) (Your local machine space)
  - \$ cd ~/tejas/docker-data
  - \$ mkdir jenkins-data
  - \$ sudo docker run --name myjenkins -p 9090:8080 -v /home/tejas/tejas/docker-data/jenkins-data:/var/jenkins_home jenkins:2.60.3
    (ERROR : not able to install any pipeline plugins bcoz -> jenkins:2.60.3 is deprecated rather use -> docker pull jenkins/jenkins)
  - \$ docker pull jenkins/jenkins:2.242
  - \$ sudo docker run --name myjenkins -p 9090:8080 -v /home/tejas/tejas/docker-data/jenkins-data:/var/jenkins_home jenkins/jenkins:2.241
  - Steps:
    - Installed Plugins
    - Create user
    - \$ docker container stop 492f49e2674d
    - \$ docker container start 492f49e2674d
    - login page (next time you login no need to re-install this plugin again)

---

- REF: https://www.jenkins.io/blog/2018/12/10/the-official-Docker-image/

---
