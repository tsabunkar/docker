Docker :

# Docker Edition :

- docker is no longer just a 'container runtime'
- docker moves fast, it matters how you install it.
- docker C.E (Community edition) -> FOSS, we do have E.E (Enterprise Edition)
- Thre major types of install : Direct, Mac/Win, Cloud
- Linux (Different per distro) (DON'T use default package)
- Docfer for windows (or Legacy Docker Toolbox)
- Docker for Mac
- DOcker for AWS/GCP/Azure (Cloud provider)

# Diff b/w C.E, EE, Stable vs Edge.

- Docker CE is free, EE is paid
- Edge -> means beta (Every Monthly)
- Stable -> Stable quaterly (Every six months)

# Docker for Windows

- Two tupes of Containers : Linux Container and Windows COntainer
- Linux Container still default, when I say "Container" I mean Linux Container.
- Docker for windows, but Wind10 pro/enterprise only.
- Wind7/8/8.1 or 10 Home should use DOcker Toolbox.

# Docker for Linux

- Docker was built natively for Linux :) thus best experience, for wind/mac they run Virtual VM of Linux behind the scene. (Kind of Emulation of Linux)
- 3 ways to install : script, store or docker-machine.

==> INSTALLING DOCKER ENGINE:

- https://docs.docker.com/install/linux/docker-ce/ubuntu/
- sudo apt-get update
- sudo apt-get upgrade
- Goto ( Install using the convenience script ) Section in https://docs.docker.com/install/linux/docker-ce/ubuntu/
- \$ curl -fsSL https://get.docker.com -o get-docker.sh
- \$ sudo sh get-docker.sh
- So if we dont want to type everytime sudo (Admin privilage) -> Thus add user account to docker group :
  \$ sudo usermod -aG docker <your-user>
  -> tejas@sabunkar:~\$ sudo usermod -aG docker tejas (to know username - whoami)
- sudo docker version

==> INSTALLING DOCKER MACHINE

- https://docs.docker.com/machine/install-machine/
- Goto If you are using Linux :

  base=https://github.com/docker/machine/releases/download/v0.16.0 &&
  curl -L $base/docker-machine-$(uname -s)-\$(uname -m) >/tmp/docker-machine &&
  sudo mv /tmp/docker-machine /usr/local/bin/docker-machine &&
  chmod +x /usr/local/bin/docker-machine

- To verify : \$ docker-machine version

==> INSTALLING DOCKER COMPOSE

- https://docs.docker.com/compose/install/
- Goto Install Compose on Linux systems :
- sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-\$(uname -m)" -o /usr/local/bin/docker-compose
- sudo chmod +x /usr/local/bin/docker-compose
- docker-compose --version

- Note : To check docker engine, machine, compose installed properly run :
  $ sudo -i (Interactive mode-> No need to append sudo = Admin Privilage granted, to quit $ exit)
  $ docker version
  $ docker-machine version
  \$ docker-compose --version

-----------------------------DONE-------------------------------

---

-> For Course git url : git clone https://github.com/BretFisher/udemy-docker-mastery.git

---

# What happen when docker version is called ?

- docker version : We get Two version Client docker version and Server docker version [basically verifies cli can talk to engine ]
- Server is also called - Engine, which runs background of local machine (NOTE: In Window it is called server, in Linux/Mac is called as daemon).
- when docker version is called : Docker CLI is talking to the server and returning its value and client value itself (Ideally both should be same, but can be of different version)

Different commands in docker CLI :

- docker info (Complete info about client and sever, Gives config values of engine)
- docker (Gives list of commands avaliable in docker)

-> Docker Command Format :

new "management commands" format :

new way > docker <command> <sub-command> (options)
old way > docker <command> (options)

---

Content :

- image v/s container
- run/stop/remove container
- check container logs and processes.

# image v/s container :

- An Image contains : binaries, libraries, source code which contributes to make up the Application. Whereas, the container is the running instance of that image.
- An Image is that application we want to run.
- A Container is an instance of that image running as a process.
- I can think my images as byte codes (1 and 0's) of particular application like- vscode, If we open/run the instance vscode images(byte code,libs, src code) as process it is called as Container.
- We can have many containers running off the same image.(Think like - we run multiple instance of vscode application in same local machine).
- Let us see how to download, install and run the Nginx Web server image as process.
- Docker default image repositry/registry is called is called DOCKER HUB (https://hub.docker.com/) [ Think it as for src code registry we have github ]

# run/stop/remove container

- docker container run -> Starts a new container from an image
- https://hub.docker.com/_/nginx
- docker container run --publish 80:80 nginx

  1. downloaded image of 'nginx' from docker hub (First check if avaliable locally in local machine or else download from docker hub registry)
  2. Started a new container from that image.
  3. Opened port 80 on the host IP.
  4. Routes that traffic to the container IP, port 80

-> Go to browser : localhost:80

NOTE: To Stop if we do - Ctrl + C , seems does not work the same way on Windows. It exits the foreground but leaves container
running in the background.

- docker container run --publish 80:80 --detach nginx
  (This will make sure the instance of nginx is running in the background)
  250745573d43affce95fe0f1fd60fa72d4868e4d4bc139ab4b4f86d5a7bb9ca2 -> (Every time we run new container, we get new Unique ID )

- docker container ls -> Shows list of container that are currently running

  - Pretty formating :
    docker container ls --format "{{.ID}}: {{.Command}}"
    docker ps --format "table {{.ID}}\t{{.Labels}}" (or) docker container ls --format "table {{.ID}}\t{{.Labels}}"

    <!-- NOTE:
     docker ps --format "table {{.ID}}\t{{.Labels}}" (or) docker container ls --format "table {{.ID}}\t{{.Labels}}"
                 ^                                                          ^
                 |__ Old Format                                            |____New Format

                 -->

    docker container ls --format "table {{.ID}}\t{{.Names}}\t{{.Image}}\t{{.Labels}}\t{{.Ports}}\t{{.Status}}"
    docker container ls --format "table {{.ID}}\t{{.Names}}\t{{.Image}}\t{{.Labels}}\t{{.Ports}}\t{{.Status}}"
    docker container ls --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"
    docker container ls --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}"

NOTE :Container Name (.Names) field is generated randomly from OSS Surname of hackers or scientist

- docker contianer stop <ContainerID> -> Stops the container process but doesnot remove it.
  ex- docker container stop 250745573d43
- run v/s stop :

  - run : '\$ docker container run' >> always starts a new container
    ex-
    \$ docker container run --publish 80:80 --detach
    \$ docker container run --publish 80:80 --detach --name ngnix_webhost nginx (Giving userdefined name)
    \$ docker container ls --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"

  - start : '\$ docker container start' >> will start an existing stopped one.

- Logs : To Show the logs of the container : docker container logs <Container_names> (.Names field)
  ex -
  $ docker container logs festive_swartz
  $ docker container logs ngnix_webhost

- To know the process which are running internal in the specific container image :
  \$ docker container top <Container_names>
- To list all the container :
  \$ docker container ls -a
- TO Remove all the container at same time : docker container rm <one_or_more_container_containerID>
  ex-
  \$ docker container rm
  \$ docker container rm b35 (only starting few character ID untill its unique from other Container ID's)
  \$ docker container rm d1b9a4122110
  (To verifiy \$ docker container ls -a)

  ERROR : 'error response from daemon you cannot remove a running container'
  \$ docker container rm -f d1b9a4122110

---

# What happens in 'docker container run' ?

1. Docker looks for the image locally in image cache, doesn't find anything.
2. THen looks in remote image repositry (default to Docker Hub).
3. Downloads the latest version
4. Create new container based on that image and prepares to start.
5. Gives it a virtual IP on a private network inside docker engine.
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image Dockerfile.

<!--
docker container run --publish 8080:80 --name webhost -d nginx:1.11 nginx -T
                                  ^                             ^     ^
                                  |                             |     |
     Change host listening port __|    Change version of image__|     |___Change CMD run on start

 -->

---

# Container v/s VirtualMachine -VM

- Containers are not mini-VM's
  - Containers are just process
  - COntainer have limited to what resources they can access
  - Container exit when process stops
  - ex- Let us download and install mongodb
    - \$ docker pull mongo
    - \$ docker run --name dock_mongo_db -d mongo
      (docker run --name <user_define_name_image> -d mongo)
    - \$ docker container ls -a
    - \$ docker top dock_mongo_db
      (List Process that running in Specific COntianer mentioned with COntainer_names )
    - \$ ps aux (or) ps aux | grep mongo
      (List of process that are currently running the local machine -> Task Manager)
      (NOte : You can find mongod is running as process in the process list, So Mongod is not hiding as VM its just like a normal process running the local machine)
    - \$ docker stop dock_mongo_db
      (Stops the docker container Instance of mongod image, Thus in the process list mongo process would be killed)
    - \$ docker start dock_mongo_db
      (This will start the mongodb instance again)

---
