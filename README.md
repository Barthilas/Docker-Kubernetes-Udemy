All docker commands where an ID can be used, you don't always have to copy / write out the full id. Just use the first (few) character(s).
docker run abcdefg
docker run abc (will also run)

Images are read-only (snapshot, closed template). To change you need to re-build again.
Docker caches every instruction result (layer based architecture). LAYER = INSTRUCTION
When one layer changes all subsequent layers also change.

Docker Run vs Start
start = run in background (does not block terminal)
run = container is running in the foreground (attached) {console log will show in terminal}.

docker --help

Build Image from dockerfile
docker build .
--tag name(repository):tag format example node:14 

Run Image (Create container)
docker run $id
Parameters:
-p 3000:80 (map port x (mine) to y (image internal port))
-i interactive
-t expose terminal by container
-it combined
-d detach (will not block terminal)
-v feedback:/app/feedback volume (named)
--rm (automatically delete container when it exits.)
--name custom name

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

Copy files to/from already running container
docker cp source/file container_name:path_inside_container

Push image to docker hub
docker login (if not)
docker push barthilas/udemy:tagname

Rename/Retag Image
docker tag oldname newname

Image vs container
container = running unit of software
images = templates for container, contains code, required tools/runtimes (one image, multiple containers/instance)

Docker hub
- search for docker images
- docker run node (create container based on image of node)
- docker run -it node

Volumes (Docker) (persist data when container shuts down)
- docker volumes ls
- managed by docker
- folders on host machine which are mounted into containers
- containers can read/write from volume
- anonymous or named (anonymous exists as long as container, removed when container stops with --rm parameters)
- Anonymous volume usage: prioritize container-internal paths higher than external paths.
- example: docker run -d -p 
3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:value  (creates named volume feedback with path /app/feedback)

Delete volume
docker volume rm VOL_NAME / docker volume prune

Create Volume
docker volume create feedback-files

Inspect volume
docker volume inspect NAME

Bind Mounts
- managed by us
- persistent and editable data (e.g source code)
- use case: provide "live data" to the container (no rebuilding needed)
- example: docker run -d -p 
3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "C:\Users\snetivy\Desktop\Docker Udemy\data-volumes-01-starting-setup":/app -v /app/node_modules  feedback-node:value
explained: you can now change server.js and see changes inside the running container because its using bind mounts to local disk. To make sure that node_modules persist we create another anonymous volume app/node_modules. (Else they would be overwritten completely by binded mount which doesnt have node_modules.)
- :ro is readonly, container can only read.

Copy vs bind mount
- bind mount will not be used in production hence why copy . . stays in dockerfile.

Arguments and Environment variables
arg: available only inside dockerfile. 
Set on image build docker build --build-arg

env: available inside docerfile and in application code.
!BEWARE OF SECURITY! KEYS, PASSWORDS ETC.
set in dockerfile or docker run --env PORT=8000
shortcut: -e
multiple -e PORT=8000 -e X=Y
OR from file: --env-file ./.env