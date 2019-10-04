# Manage Multiple Container

- Explore https://docs.docker.com/
- Run a nginx, mysql and httpd (apache) server
- Run all of them --detach (-d), name them with --name
- nginx shld listen on 80:80, httpd on 8080:80, mysql on 3306:3306
- when running mysql, use the --env (-e) to pass in MYSQL_RANDOM_ROOT_PASSWORD=yes
- Use docker container logs on mysql to find the random password it created on startup.
- Clean it all up with docker container stop and docker container rm (Accepts .Names)
- Use docker container ls -a to ensure everything is cleaned up.

---

# My-Solution

- \$ docker pull mysql
- \$ docker pull httpd
- I already have nginx
  - To Check weather you if Image is present locally : \$ docker image inspect myimage:mytag
    ex- docker image inspect mongo or docker image inspect mongo:latest
    docker image inspect mysqlw
    ( Note : If you get Error: No such image: mysqlw, that means locally you don't have that image So need to pull from Docker Hub)
- \$ docker run --name mysql-db -e MYSQL_RANDOM_ROOT_PASSWORD=yes -d mysql:latest
  \$ docker container ls -a
- \$ docker run --name apache-httpd -p 8080:80 -d httpd:latest
  \$ docker container ls -a
  Goto Browser : http://localhost:8080/
- \$ docker run --name nginx-server -p 80:80 -d nginx:latest
  \$ docker container ls -a
- \$ docker container logs mysql-db
- \$ docker container stop nginx-server apache-httpd mysql-db
  \$ docker container ls -a
- \$ docker container rm nginx-server apache-httpd mysql-db
  \$ docker container ls -a

---

# Actual-Solution

- \$ docker run --name mysql-db -p 3306:3306 -e MYSQL_RANDOM_ROOT_PASSWORD=yes -d mysql:latest
- \$ docker container logs mysql-db
- \$ curl localhost:8080 (This will show it works in console, instead of verifying in the browser)
- \$ docker image ls
  (This will list all the image installed in my local machine)
