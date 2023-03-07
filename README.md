All docker commands where an ID can be used, you don't always have to copy / write out the full id. Just use the first (few) character(s).
docker run abcdefg
docker run abc (will also run)

Images are read-only (snapshot, closed template). To change you need to re-build again.
Docker caches every instruction result (layer based architecture).
When one layer changes all subsequent layers also change.

Docker Run vs Start
start = run in background (does not block terminal)
run = container is running in the foreground (attached) {console log will show in terminal}.

docker --help

Build Image from dockerfile
docker build .

Run Image (Create container)
docker run $id
Parameters:
-p 3000:80 (map port x (mine) to y (image port))
-i interactive
-t expose terminal by container
-it combined
-d detach (will not block terminal)
--rm (automatically delete container when it exits.)

Restart container
docker start
-a attached mode (will block terminal)
-i interactive

Show running dockers
docker ps
-a all(default is running)

Show images
docker images

Stop container
docker stop $name

Attach to detached container
docker container attach CONTAINER_NAME

Fetch logs
docker logs CONTAINER_NAME/ID
-f follow (listen future logs)

Remove containers
docker rm CONTAINER_NAME NAME2...
docker container prune (remove all stopped containers at once)

Remove images
docker rmi ID ID2...
docker images prune (remove all images)

Inspect images
docker image inspect IMAGE_NAME

Image vs container
container = running unit of software
images = templates for container, contains code, required tools/runtimes (one image, multiple containers/instance)

Docker hub
- search for docker images
- docker run node (create container based on image of node)
- docker run -it node