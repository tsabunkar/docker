# Running Docker as GUI/ application

- For linux : --net=host -e DISPLAY=:0
- \$ docker run --rm -ti --net=host -e DISPLAY=:0 fr3nd/xeyes
- If I want to run tor browser in my machine without actually installing it, First find the image of tor browser in dockerhub (instead of pulling and then runninng, run directly- which will try find locally first if not found then install from central repo and then run)
- \$ docker run --rm -ti --net=host -e DISPLAY=:0 jess/firefox

REFERENCE : https://medium.com/better-programming/running-desktop-apps-in-docker-43a70a5265c4

---
