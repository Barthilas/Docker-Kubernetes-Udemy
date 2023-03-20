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
Set on image build docker build --build-arg DEFAULT_PORT=8000

env: available inside docerfile and in application code.
!BEWARE OF SECURITY! KEYS, PASSWORDS ETC.
set in dockerfile or docker run --env PORT=8000
shortcut: -e
multiple -e PORT=8000 -e X=Y
OR from file: --env-file ./.env

Container communication
- request inside the container to www works without any special settings.
- inside container to local machine: replace localhost with special domain"host.docker.internal"
- container to container -> container networks (docker run --network my_network...)

Container Network
- docker network create favorites-net
- docker network ls
- docker run -d --name mongodb --network favorites-net mongo 
- docker run --name favorites --network favorites-net -d --rm -p 3000:3000 favorites-node

Docker Compose: Multi-Container Orchestration
- solves too much noise when setting up three containers working together (named volumes, bind mounts, networks)
- automate multi-container setups
- will not replace dockerfiles
- does not replace images or containers
- not suited for managing multiple containers on different hosts
- can be used for single containers as well. (Volumes are easier)
- docker-compose up
  - d detached
- docker-compose down
  - v delete volumes

"Utility Containers" (unofficial naming)
docker exec -it container_name npm init
docker run -it node npm init (override default command)

Dockers deployment
- bind mounts shouldnt be used.
- containerized apps might need a build step (e.g. Angular)
- multi-container projects might need to be split across multiple hosts.
- trade-offs between control and responsibility.
- without kubernetes or similiar locked to service provider deployment process/specifications (AWS, Azure)!


Kubernetes
- de facto standard for orchestrating container deployments (without locking to one service like AWS, Azure..)

Core Concepts
- Pod (Container) - usually not created by you but by Deployment object.
- Worker Node - runs container of your application, machines/virtual instances.
- Master Node - The Control Plane - controls worker nodes
- Cluster - contains all above.

- minikube start --driver=docker
- minikube status
- minikube dashboard

Deploy imperative approach
kubectl help
kubectl create deployment first-app --image=kub-first-app
kubectl get deployments
kubectl get pods
kubectl delete deployment first-app
(fails because image does not exist locally on minikube -> push to docker hub)
docker tag kub-first-app barthilas/kub-first-app
docker push barthilas/kub-first-app
(try again - success)
kubectl create deployment first-app --image=barthilas/kub-first-app
(use "service" object to reach pod, otherwise unaccessible from outside.)
kubectl expose deployment first-app --port=8080 --type=LoadBalancer
kubectl get services
minikube service first-app (not needed on deployed instance as external ip should be filled by loadBalancer, on localhost we use minikube command)

Scaling
kubectl scale deployment/first-app --replicas=3
kubectl scale deployment/first-app --replicas=1

Update deployment (you need to change tag)
kubectl set image deployment/first-app kub-first-app=barthilas/kub-first-app:2
kubectl rollout status deployment/first-app

Rollback
kubectl set image deployment/first-app kub-first-app=barthilas/kub-first-app:3 (doesnt exist)
kubectl rollout status deployment/first-app (app will still run as pod will be terminated only when the new pod is up and running successfully)
kubectl rollout undo deployment/first-app
(kubectl rollout undo deployment/first-app --to-revision=1)

History
kubectl rollout history deployment/first-app 
kubectl rollout history deployment/first-app --revision=2

Docker declarative (comparable to docker compose)
kubectl apply -f=deployment.yaml
kubectl delete -f=deployment.yaml,service.yaml
kubectl delete deployments,services -l group=example
