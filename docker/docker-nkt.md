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
