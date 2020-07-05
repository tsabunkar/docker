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

# Create Docker Custom Image

- https://github.com/tsabunkar/docker-webapp-flask.git

- MANUAL STEPS:

  - \$ cd /home/tejas/tejas/workspace/vsc/docker/docker/py-web-app
  - \$ docker run -it ubuntu:20.10 bash
  - \$ # apt-get update
  - \$ # apt-get install -y python3.6
  - \$ # apt install -y python3-pip
  - \$ # pip install flask
  - \$ # apt-get -y install nano
  - \$ # nano /opt/app.py (copy source code from [./py-web-app/app.py])
  - \$ # cd /opt
  - \$ FLASK_APP=app.py flask run --host=0.0.0.0
  - (Visit -> http://172.17.0.2:5000/ ) {To know ip addr do docker inspect of containerID}

- AUTOMATE ABOVE STEPS USING Dockerfile:

  - \$ cd /home/tejas/tejas/workspace/vsc/docker/docker/py-web-app
  - \$ docker build . (run build command where you have Dockerfile)
  - \$ docker build . -t tsabunkar/py-webapp
  - \$ docker images ls (New image would have been created as - 'tsabunkar/py-webapp')
  - \$ docker run tsabunkar/py-webapp (Run custom image as container/instance)
  - (Visit -> http://172.17.0.3:7575/ )

---

# To Push cutom image to DockerHub Registry

- \$ docker login
- \$ docker push tsabunkar/py-webapp (please NOTE: image name should be -> tsabunkar/<img> preffix with username)

---

# Docker Compose- Manual Steps (Voting APP)

- git clone https://github.com/tsabunkar/docker-voting-app.git

- (Build images of voting-app, worker, result-app and push to your dockerhub repo )
  - (voting-app) (Python based Web app)
    - \$ cd /home/tejas/tejas/workspace/vsc/docker-voting-app/vote (find Dockerfile)
    - \$ docker build . -t tsabunkar/voting-app (build custom image of Python based Web App)
    - \$ docker run -p 5000:8080 tsabunkar/voting-app (running instance of this image)
    - (visit : http://172.17.0.2:8080/ [internal container IpAddr] or http://172.17.0.1:5000/ [docker Host ipAddr])
    - (cast vote -> ERROR : 500 Internal server error)
  - \$ docker run --name=redis -p 6379:6379 redis:6 (Running Redis DB from 3rd party)
  - \$ docker run -p 5000:8080 --link redis:redis tsabunkar/voting-app (linking Redis in-memory db with python web app) (remove redis_password as there is no password require for redis cache)
- \$ docker run --name=db -e POSTGRES_HOST_AUTH_METHOD='trust' \
  -e POSTGRES_USER='postgres' \
  -e POSTGRES_PASSWORD='postgres' \
  postgres:9.4
  (Running postgres DB from 3rd party) (envrionment variable -> POSTGRES_HOST_AUTH_METHOD=trust To trust & allow all connection)
- (building worker app -> .net framework app)
  - \$ cd /home/tejas/tejas/workspace/vsc/docker-voting-app/worker
  - \$ docker build . -t tsabunkar/worker-app
  - \$ docker run --link redis:redis --link db:db tsabunkar/worker-app (linking worker app to redis and postgres db)
- (building result app -> nodejs app)
  - \$ cd /home/tejas/tejas/workspace/vsc/docker-voting-app/result
  - \$ docker build . -t tsabunkar/result-app
  - \$ docker run -p 5001:80 --link db:db tsabunkar/result-app (Docker Host Ip addr - 5000 and Internal ip addr of container - 80 or 8080 ) (link result app with postgres db)
- (Result Page -> http://172.17.0.1:5001/)

---

# Docker Compose- Automate Steps using docker-compose.yml file (Voting APP)

- Install docker compose in your machine, before starting -> https://docs.docker.com/compose/install/
- Stop all containers and remove those containers
- (inside this docker-compose.yml file define sevices)
- \$ cd /home/tejas/tejas/workspace/vsc/docker-voting-app
- \$ docker-compose -f docker-compose-2.yml config (Validate docker-compose file sytnax)
- \$ docker-compose -f docker-compose-2.yml up
- \$ docker-compose -f docker-compose-2.yml down

---

# Push this image created

- docker push tsabunkar/voting-app
- docker push tsabunkar/worker-app
- docker push tsabunkar/result-app
