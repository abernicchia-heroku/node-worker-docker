# Background Jobs in Node.js with Redis

Redis-backed background worker example using [OptimalBits/bull](https://github.com/OptimalBits/bull) and [throng](https://github.com/hunterloftis/throng).

## Docker setup
Use the following commands to create a set of containers:
- exposing a port to interact with from the Host
- using a custom network bind to connect the Redis container using its name (the default bridge network does not allow this and a container cannot bind the Host's localhost from Mac see https://docs.docker.com/docker-for-mac/networking/#use-cases-and-workarounds)
- overriding the $REDIS_URL with the Redis container URL

```
 docker run -d --network=my-docker-net -p 6379:6379 --name redis <image name - e.g. redis:5.0.8-alpine>
 docker run -d --network=my-docker-net -p 5000:5000 --name nodeserver --env REDIS_URL=redis://redis:6379 <image name - e.g. node-server>
 docker run -d --network=my-docker-net --name nodeworker --env REDIS_URL=redis://redis:6379 <image name - e.g. node-worker> 
```

## Application Overview

The application is comprised of one process: 

- **`worker`** - A small node process that listens for and executes incoming jobs