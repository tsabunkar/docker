# Dockerizing a Node.js web app

- create node-server
- create docker file
- docker build -t tejas/node-web-app .
- docker images

---

# Run the image

- docker run -p 4900:8080 -d tejas/node-web-app
- Go inside container : \$ docker exec -it <container id> /bin/bash
- Call the App : \$ curl -i localhost:4900

---

Reference : https://nodejs.org/de/docs/guides/nodejs-docker-webapp/

Force Remove docker image : \$ docker image rm --force <image_ID>
