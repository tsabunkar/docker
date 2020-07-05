# What am I Containerizing? (Why to create an image ?)

- You cannot find the stack of application in docker hub as required with paticular configuration
- Your application will be dockerized from Dev team to Ops team, in ease of shipping and deployment

---

# How to create my own image ?

- Think of steps you will create will deploying an application manually
- Examle:
  - OS : Ubuntu
  - Update apt repo
  - Install dependencies using apt
  - Install python dependencies using pip
  - Copy source code to /apt folder
  - Run the web server using "flask" command
- Now once we know above steps --convert_to--> Dockerfile
- Dockerfile

```
FROM Ubuntu

RUN apt-get update
RUN apt-get install python

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run

```

- Build the above docker file using command (create an image locally with name: 'tsabunkar/my-py-webapp') -> \$ docker build Dockerfile -t tsabunkar/my-py-webapp
- To push this image in to docker hub, so others can access it -> \$ docker push tsabunkar/my-py-webapp

---

# Dockerfile

- Dockerfile is a tex file, written specific INSTRUCTIONS and ARGUMENT format which docker can understand
- INSTRUCTIONS in Dockerfile are- FROM, RUN, COPY, ENTRYPOINT (left side of file)
  - This is will instruct docker to perform specific action while creating the image
- ARGUMENT are in right side of Dockerfile- (linux commands)
- FROM instruction tells Docker it is base OS or another image which this will be mounted upon
  - FROM instruction are first line of Dockerfile
- RUN instruction- install all dependencies
- COPY instruction- copy local files from machine into docker image
- ENTRYPOINT instruction- specify Entrypoint to be run when this image is run as container

---

# Layered Architecture

- When docker builds up an image it builds in layered architecture
- Each line of instruction will create new layer in docker image
- [.assets/layered-arch.png]

---

# Docker build output

- When we run docker build command, we can see various Steps Run and result of each task
- Here layer arch helps- as if particular step fails no need to start from begining, rather just before the Step it failed (bcoz - all layer builds are Cached) Thus, rebuilding process is faster

---

# What can you containerize

- chrome, morizilla, curl, skype, spotifiy
- "CAN Containerize Everthing"

---

# Environment Variables

- [./assets/env-variable.png]
- To pass environment variable as argument in docker run command -> \$ docker run -e APP_COLOR=blue tsabunkar/py-webapp
- How to know the environment variable -> \$ docker container inspect <container_id> (under "Config": { "Env" } )

---

# docker CMD v/s Entrypoint

- Container are only meant to run process, task, db instances, web server, computational logic, It will be alive only uptill the process/web-server inside the container is running or else container will be exited
- How define, what process to run inside the container ? --> CMD instruction
- CMD instruction : Define the program to be run within the container when it start
- For ex-
  - nginx Dockerfile -> CMD ["nginx"]
  - mysql Dockerfile -> CMD ["mysqld"]
  - custom Dockerfile -> CMD ["bash"] (but bash is not process like - webserver/appserver/dbserver but a shell which listens from input of a terminal)
- We can override Dockerfile image CMD by specifying in docker run command i.e-
  - \$ docker run ubuntu:20.10 [COMMAND]
  - \$ docker run ubuntu:20.10 sleep 1000
- How to make above command permanet ?
  - Specifiy in the CMD instruction in Docker file
  - Syntax: \$ CMD command param1 ==> CMD ["command", "param1"]
  - Example: \$ CMD sleep 5 ==> CMD ["sleep", "5"]
- If we want to not specify params in docker run command the need to specifiy in ENTRYPOINT instruction --> [./assets/entrypoint.png]
- What if don't specifiy ENTRYPOINT value in the docker run command ? then need to specifiy default value with CMD instruction. Therefore need to specifiy both instructions : ENTRYPOINT and CMD --> [./assets/both-instruction.png]
