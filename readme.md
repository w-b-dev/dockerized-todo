# instructions

TODO:

- understand about layers and caching

    Going back to the image history output, we see that each command in the Dockerfile becomes a new layer in the image. You might remember that when we made a change to the image, the yarn dependencies had to be reinstalled. Is there a way to fix this? It doesn't make much sense to ship around the same dependencies every time we build, right?

    To fix this, we need to restructure our Dockerfile to help support the caching of the dependencies. For Node-based applications, those dependencies are defined in the package.json file. So, what if we copied only that file in first, install the dependencies, and then copy in everything else? Then, we only recreate the yarn dependencies if there was a change to the package.json. Make sense?

## Basic run
Go for `docker-compose up -d`

    By default, named volumes in your compose file are NOT removed when running `docker-compose down`. 

Initially I was using `docker run -d -p 3000:3000 -v todo-db:/etc/todos  getting-started`

## Build

```
docker build -t getting-started .
```

## Push

```
docker push brasileiro/getting-started:latest
```

## DEV: Connect and bind to source code

```
docker run -dp 3001:3000 \
    -w /app -v "$(pwd):/app" \
    node:12-alpine \
    sh -c "yarn install && yarn run dev"
```

## Others

How I created a named volume
```
docker volume create todo-db
```

How to list running instances
```
dockers ps
```

How to run a bash script in a brand new image
```
docker run -d ubuntu bash -c "shuf -i 1-10000 -n 1 -o /data.txt && tail -f /dev/null"
```

How to create a network
```
docker network create todo-app
```

How to run a MySQL instance connected to the network
    
- Note: Docker recognizes we want to use a named volume and creates one automatically for us.
```
docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:5.7
```

- To connect to the db (get instance with `docker ps`)
```
docker exec -it <mysql-container-id> mysql -p
```