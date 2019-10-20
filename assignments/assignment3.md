# Assignment: DNS Round Robin Test

- Ever since Docker Engine 1.11, we can have multiple containers on a created network respond to the same DNS address
- Create a new virtual network (default bridge driver)
- Create two containers from 'elasticsearch:2' image
- Research and use '--net-alias search' when creating them to give them an additional DNS name to respond to
- Run 'alpine nslookup search' with '--net' to see the two containers list for the same DNS name
- Run 'centos curl -s search:9200' with '--net' multiple times with until you see both 'name' fields show

---

- \$ docker network create my_network
- \$ docker network ls
- \$ docker pull elasticsearch:5.6.15
- \$ docker container run -d --net my_network --net-alias search elasticsearch:5.6.15 ==> Create a container of elasticsearch in the network called -> my_network [Container name not given]
- \$ docker container run -d --net my_network --net-alias search elasticsearch:5.6.15 ===> Creating second instance of elasticsearch container
- \$ docker container ls -a
- \$ docker container run --rm --net my_network alpine nslookup search ==> Run search lookup in ns dns entry and then immediatley exit by cleaning itself bcoz --rm flag
- \$ docker container run --rm --net my_network centos:7 curl -s search:9200
- \$ docker container run --rm --net my_network centos:7 curl -s search:9200
- \$ docker container ls
- \$ docker container rm -f <image_name>
