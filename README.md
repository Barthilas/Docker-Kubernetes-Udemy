Build Image from dockerfile
docker build .

Run Image
docker run $id
Parameters:
-p 3000:80 (map port x (mine) to y (image port))
-it interactive

Show running dockers
docker ps
-a all(default is running)

Stop container
docker stop $name

Image vs container
container = running unit of software
images = templates for container, contains code, required tools/runtimes (one image, multiple containers/instance)

Docker hub
- search for docker images
- docker run node (create container based on image of node)
- docker run -it node