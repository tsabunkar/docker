# Docker Engine

- Docker Engine Consist of :
  - Docker CLI (Command line interface which uses REST API to interact with Docker Deamon)
  - REST API (API interface for docker deamon)
  - Docker Deamon (background process which manage object like - image, containers, volumne, networks, etc )
- [.assets/docker-engine.png]
- NOTE: docker CLI can be in different host/machine --> need to provide -H
  - [.assets/docker-engine-2.png]

---

# Containerization

- Docker uses namespaces to isolate workspace
- ProcessID, Unix Timesharing, Network, Mount, InterProcess are created in own namespace thereby- providing isolation b/w containers
- [.assets/containerization.png]
- One of the isolation techinque : PID Namespace (Process ID)
  - each process running in the container are nothing but process running in the host with different process ID
  - [.assets/namespace-pid.png]
- cgroups
  - docker manages to provide feature of shared resources b/w containers
  - bydefault container can use Atmost CPU and memory of host machine but we can restrict this usage of CPU and m'my of host machine by container using -> cgroup (controlled Groups)
  - cgroups to restrict amount of h/w resources allocated to each container
    - To Restrict CPU : \$ docker run --cpu=.5 ubuntu (container cannot use more than 50% of host machine CPU)
    - To Restrict Memory : \$ docker run --memory=100m ubuntu (container can use max of 100 MB memory)

---

# Docker Storage

- Docker Store data in Local file system
  - When docker is installed, it creates folder : /var/lib/docker/
- wkt, docker uses layered architecture i.e- when an image is build -> each line of instruction in Dockerfile creates a new layer in the docker image, each layer is cached, also all layer are sequential and depends on previous layer
  - [.assets/layered-arch-cache.png]
- Think Docker image can have abstractly 2 layers: [.assets/2-layer-arch.png]
  - Image Layer:
    - Which is Read-only
    - Creates during image is build from Dockerfile
    - This layer is re-used every time if a new instance(container) is created from same image
  - Container Layer:
    - both Read & write
    - creates an image is run
    - lifecycle of this layer is only from docker run to docker stop
    - If we create a file inside the container it will be inside container layer ex- temp.txt
    - If we want to modify a file of image layer suppose app.py then a particular container intance, then we can still do that bcoz- docker always create a copy of all files of Image layer into Container Layer, thus called COPY-ON-WRITE mechanism (NOTE: file inside the Image layer will never be modified, rather the copy of this file present in Container layer will be modified) [.assets/copy-on-write.png]
- Volumnes
  - When container is stopped and removed all files present in Container Layer will be lost permanently.
  - Inorder to persist data/file of container layer outside the container scope we can create volume link to outside Docker Host
  - \$ docker volume create data_volume (Create volume in host machine inside --> /var/lib/docker/)
  - \$ docker run -v data_volume:/var/lib/mysql mysql (Providing link to store data present in mysql container to outside Docker Host dir/volume)
  - 2 Types of Volume Mounting in Docker
    - \$ docker run -v data_volume2:/var/lib/mysql mysql (if we dont create dir data_volume2 explicitly then docker will create this for us :) ==> called "VOLUME MOUNTING"
    - \$ docker run -v /tejas/mysql-data:/var/lib/mysql mysql (here we explicitly creates dir /tejas/mysql-data and then docker will mount/link content inside mysql container in it) ==> called "BIND MOUNTING"
    - -v flag is depercated rather use --> --mount flag
      - \$ docker run -v /tejas/mysql-data:/var/lib/mysql mysql
      - (Above command can be written as)
      - \$ docker run \ --mount type=bind,source=/tejas/mysql-data,target=/var/lib/mysql mysql
  - [.assets/volumes.png]
- Docker uses storage driver inorder to mantaine Layered Architecture
- Common Storage drivers are : AUFS, ZFS, BTRFS, Device Mapper, Overlay, Overlay2
- Selection of storage divers depends upon host machine OS type i.e- for Ubuntu -> AUFS, Fedora -> Device Mapper
