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
- ENTRYPOINT instruction- specify Entrypoint to be run when this image is run as container (entry file name)

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
