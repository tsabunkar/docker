# this same shows how we can extend/change an existing official image from Docker Hub

FROM nginx:latest
# highly recommend you always pin versions for anything beyond dev/learn

WORKDIR /usr/share/nginx/html
# change working directory to root of nginx webhost
# using WORKDIR is preferred to using 'RUN cd /some/path'

COPY index.html index.html

# I don't have to specify EXPOSE or CMD because they're in my FROM


# Steps :
# go to this location -> tejas/workspace/vsc/docker/docker/container-images/docker-file/file2
# $ docker container run -p 80:80 --rm nginx
# $ docker build -t nginx-wth-html .
# $ docker image ls
# $ docker container run -p 80:80 --rm nginx-wth-html
# IF you want to remove this image : docker image rm nginx-wth-html:latest