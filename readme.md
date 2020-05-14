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
  (https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script)

- \$ sudo curl -fsSL https://get.docker.com -o get-docker.sh
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

# Install docker in ubuntu LTS 20.04

- Currently Docker does not support Ubuntu LTS 20.04 So we need to using bionic (Ubuntu 18.04 LTS) release file to install docker or use old way of installing docker i.e.:-

- sudo apt install docker.io
- NOTE : Steps remain same to install docker compose

Reference : https://askubuntu.com/questions/1230189/how-to-install-docker-community-on-ubuntu-20-04-lts

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

# What is going on in container :

- docker container top - list of process which are running in that container specified.
  - \$ docker container top nginx-server
- docker container inspect - details of one container config
  - \$ docker container inspect mysql-db
- docker container stats - performance stats for all containers.
  - \$ docker container stats

---

# Getting a shell inside container

- docker container run -it -> Start new container interactively

  - Allows developer to go into the container and perform application CLI specific to application.
  - No SSH is needed for interacting with your application (DOcker CLI is great substitute for adding SSH to containers)
  - \$ docker container run -it --help
    (t -> gives us pseudo-TTY [ Which simulates a real terminal, like a how SSH does])
    (i -> Keeps session open to receive terminal input)
  - docker container run -it --name <.Names> nginx bash
    (if run with -it, This will provide us a terminal/shell inside the running container)
    ex- \$ docker container run -it --name nginx-server2 nginx bash

- To start/re-run the image in terminal for OS :
  ex - \$ docker container start -ai ubunutu

- To start/re-run the image in terminal for Application:
  docker container exec -it -> run additional command in existing container
  ex- \$ docker container exec -it mysql-db bash

- NOTE : We can isntall another OS inside the docker, let us install alpine which is smallest OS of linux distro that have size of 5MB
  - \$ docker pull alpine
  - \$ docker image ls -a
  - \$ docker container run -it alpine bash (ERROR : alpine does not have bash, so use sh [Also alpine package manager is apk]) -> \$ docker container run -it alpine sh
    \$ echo 'hello'

---

# Docker Networks

- docker container run -p ==> Expose the port
- for local dev/testing, network usually 'just work'
- quick port check with '\$ docker container port < container >'
- When we start container, we are at the background to connect to particular docker network- which is called network bridge
- Each container connected to a private virtual network 'bridge'.
- Each virtual network routes through NAT firewall on host IP.
- All Container on a virtual network can talk to each other without -p (port)
- When we have an application which has apache server container and node server container, those two container should be in same network, they will be able to talk to each other without actually having to expose port and trying connect to each others port.
- Best pracitce : is to create a new virtual network for each app :
  - network 'my_web_app' for mysql and apache containers.
  - network 'my_api' for mongo and nodejs containers.
- BATTERIES INCLUDED, BUT REMOVABLE
  - default work well in many cases, but easy to swap out parts to customize it.
- Make new virtual networks.
- Attach containers to more then one virtual network (or none).
- Skip virtual networks and use host IP (--net=host)
- Use different Docker network drivers to gain new abilities.

Some cmds :

- \$ docker container run -p 80:80 --name nginx-webhost -d nginx
  (-p or --publish ==> publishing port is always in HOST:CONTAINER format)

- \$ docker container port nginx-webhost
  (TO get the port number)
- \$ docker container inspect --format '{{.NetworkSettings.IPAddress}}' nginx-webhost
  (Shows the containers IP Address)

---

# Docker Network : CLI

- Show networks : docker network ls

  - \$ docker network ls : All the network that are created
  - (--network bridge :- Default docker virtual network, Which is NAT'ed behinf the Host IP)
  - (--network host :- It gains performance by skipping virtual networks but sacrifies security of container models)
  - (--network none :- removes eth0 and only leaves you with localhost interface in container)

- Inspect a network : docker network inspect

  - \$ docker network inspect bridge => Inspecting specific network, in this case bridge network

- Create a network : docker network create --driver

  - \$ docker network create my_app_net => Spawns a new virtual network for you to attach container to
  - \$ docker network ls => Default network driver create is bridge (Network driver : Built-in or 3rd party extension that give you virtual network features)
  - \$ docker container run -d --name nginx_webserver --network my_app_net nginx => Creating new nginx_webserver contianer in network which was previously created my_app_net

- Attach a network to container : docker network connect

  - \$ docker network connect < ContianerID > => Dynamically creates a NIC in a container on an exiting Virtual network

- Dettach a network from container : docker network disconnect

- docker network disconnect < ContianerID > => Dynamically removes a NIC from a container in a specific VM.

---

# Docker Networks : Default Security

- Create your apps so frontend/backend sit on same docker network.
- Their inter-communication never leave host.
- All externally exposed ports closed by default.
- You must manually expose via -p, which is better default security!
- This gets even better later with Swarm and Overlay networks

---

# Docker Networks : DNS

- Understand how DNS is the key to easy inter-container comms.
- See how it works by default custom networks
- Learn how to use --link to enable DNS on default bridge network

* Note : Forget IP's - Static IP's and using IP's for talking to containers is an anti-pattern. Do your best to avoid it

- DNS Defautl Names : Docker defaults the hostname to the container's name, but you can also set aliases
- DNS Naming : Docker DNS - Docker daemon has a built-in DNS server that containers use by default

- Cmds :

  - \$ docker container run -d --name my_nginx --network my_app_net nginx
  - \$ docker network ls (from networkID column copy the networkId of my_app_net network ex- 64fd52b00798)
  - \$ docker network inspect <NetworkID>
    Ex- \$ docker network inspect 64fd52b00798 ==> Inspecting the my_app_net network, where we can see the list of container this network is having in 'Container' property.
  - \$ docker container run -d --name my_new_nginx --network my_app_net nginx
  - \$ docker network inspect 64fd52b00798
    ( Now you will find the 'Containers' property has two container in my_app_net network i.e- my_nginx and my_new_nginx, NOTE : both the [my_nginx, my_new_nginx] container has different IPv4 address in the same network [ my_app_network] ).
  - Go inside bin/bash directory of my_nginx container :
    - \$ docker container exec -it my_nginx /bin/bash
    - \$ apt-get update
    - \$ apt-get install iputils-ping
    - \$ docker container exec -it my_nginx ping my_new_nginx
      (Thus two different Containers/i.e- Different instance of image is communicating with each-other in same network- my_app_net)

- Thus to Conclude:
  - Containers Shouldn't rely on IP's for inter-communication
  - DNS for firendly names is builtin if u can use custom networks
  - In future scope we will see Cotainer communication over a network gets easy with the concept of -> DOCKER COMPOSE

---

# Further topic discussed :

- Container Images : docker/container-images
- Container Lifetime & Presistent Data volumes : docker/container-lifetime
- Docker Compose : docker/compose
- Swarm : swarm
- Swarm Lifcycle : swarm/life-cycle
- Container Registries Image Storage and Distribution :

---

# To remove old Docker containers

- docker container stop <img_id> ..... <img_id>
- docker container prune
- docker system prune
- docker ps --filter "status=exited" | grep 'weeks ago' | awk '{print \$1}' | xargs --no-run-if-empty docker rm

REF:
https://stackoverflow.com/questions/17236796/how-to-remove-old-docker-containers

---
