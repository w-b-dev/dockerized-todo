# instructions

## Basic run

```
docker run -d -p 3000:3000 -v todo-db:/etc/todos  getting-started
```

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