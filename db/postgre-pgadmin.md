# Download and Install PostgreSQL

## Install PostgreSQL database service

- Enable PostgreSQL Apt Repository
  - Syntax : deb http://apt.postgresql.org/pub/repos/apt/ YOUR_UBUNTU_VERSION_HERE-pgdg main
    - deb http://apt.postgresql.org/pub/repos/apt/ focal-pgdg main
    - sudo nano /etc/apt/sources.list
    - Then add above deb line : deb http://apt.postgresql.org/pub/repos/apt/ focal-pgdg main
  - wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - sudo apt-get update
- Install PostgreSQL on Ubuntu
  - sudo apt-get install postgresql-12
  - psql --version
- Connect to PostgreSQL
  - sudo -i -u postgres (login as postgress as user)
  - psql (Open interactive Terminal)
- Log in to the cluster
  - ps -ef | grep postgres
- Remove PostgreSQL
  - sudo apt-get --purge remove postgresql
  - dpkg -l | grep postgres
  - sudo apt-get --purge remove postgresql postgresql-doc postgresql-common
  - sudo rm -rf /var/lib/postgresql/
  - sudo rm -rf /var/log/postgresql/
  - sudo rm -rf /etc/postgresql/
  - sudo deluser postgres

Reference :
https://www.postgresql.org/download/
https://www.enterprisedb.com/postgres-tutorials/how-install-postgres-ubuntu
https://www.w3resource.com/PostgreSQL/connect-to-postgresql-database.php
https://askubuntu.com/questions/32730/how-to-remove-postgres-from-my-installation

## Install PostgreSQL database service using docker

- Install docker in Ubunut focal 20.04
  - sudo apt install docker.io
  - sudo docker pull hello-world
  - sudo docker run hello-world
  - sudo docker images hello-world
- docker pull postgres:12
- docker image ls
- docker run --name postgres-server -e POSTGRES_PASSWORD=root -d postgres:12
- docker container ls
- docker container stop postgres-server
- docker container rm postgres-server
- docker exec -it postgres-server bash
- psql --version
- psql -U postgres [bcoz- postgresSQL create default user called postgres]
- SELECT 1;

REF:
https://hub.docker.com/_/postgres

## Install PgAdmin

- docker pull dpage/pgadmin4:4.21
- docker run -p 90:9002 \
   -e 'PGADMIN_DEFAULT_EMAIL=tsabunkar@gmail.com' \
   -e 'PGADMIN_DEFAULT_PASSWORD=root' \
   -d dpage/pgadmin4:4.21
- (To know what is the ip address and port number, Inspect the container)
  sudo docker inspect <container_id>
- "IPAddress": "172.17.0.3",
- User name: tsabunkar@gmail.com
  password: root
- server > create > Server
- Name : postgre-test
  Connection : 172.17.0.2 (This ip address you will know, by inspecting postgre-server container running)
  Port : 5432
  Maintance db : postgres
  Username : postgres [default user provide by postgreSQL]
  password : (you know)

REF:
https://www.pgadmin.org/docs/pgadmin4/latest/container_deployment.html

- NOTE: To start the stop containers : sudo docker container start a842936f7ad0 7ca6eee00427

---
