# Without docker network
docker run --name mongodb --rm -d -p 27017:27017 mongo

# Preserve data with docker network
docker run --name mongodb --rm -d -p 27017:27017 -network goals-net -v data:/data/db mongo

# Mongodb authentication (from docs)
docker run --name mongodb --rm -d -p 27017:27017 -network goals-net -v data:/data/db
-e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=secret
mongo