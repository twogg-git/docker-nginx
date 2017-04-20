![deploying-local-docker](docker.png)
## Expose a container, build and run an image locally

### Building an image
We are going to use a super small Linux distribution called Alpine Linux with an Nginx HTTP server. 
Inside this folder we have a file called "Dockerfile" that is the base of our image.

Line 1 used `FROM` directive that references an existing image in the registry.
Line 2 used `COPY` directive to copy the contents of the `./site` folder to `/usr/share/nginx/html` inside of the container.

### Running an image

In order to run this image, we first build it :

```sh
$ docker build -t my-nginx .
```

Building an image, executes every statement inside of the `Dockerfile` and overlays the resulting file system on top of the previous one.

We are going to use `-p 3001:80` flag to maps the port 3001 on the host machine to the port 80 inside the container.

```sh
$ docker run -p 3000:80 my-nginx
```

We can test our docker container running on:

```sh
http://localhost:3000
```

Next step is working with a *volumne*. We are going to use the html file located inside site-volume folder, so each time we run our docker container it will refresh the changes in our page. (Don't forget to change the folder's path).

```sh
docker run -p 3000:80 -v /__YOUR-FOLDER-PATH__/local-docker/site-volume:/usr/share/nginx/html my-nginx 
```

Now chage the content of site-volume/index.html file and run again the previous command, the changes will loaded by the docker container.

Finally we are going to use `-d` flag to run our docker as "daemonized" container "Run in background".

```sh
$ docker run -d -p 3000:80 my-nginx
```

### Useful Docker Commands 


Obtaining a list of currently running containers
```sh
$ docker ps
```

Obtaining a list of all containers running / stopped
```sh
$ docker ps -a
```

Create and start a container 
```sh
$ docker run -it <idcontainer>
```

Create and start a container in background
```sh
$ docker run -d <idcontainer>
```

Create and start a container with port map from local to container
```sh
$ docker run -it -p <localport>:<containerport> <idcontainer>
```

Create and start, but if the container is stop it will be removed 
```sh
$ docker run --rm <idcontainer>
```

Stopping a container
```sh
$ docker stop <container name>
```

Removing a container (has to be stopped)
```sh
$ docker rm <container name>
```

Removing a container (has to be stopped)
```sh
$ docker rm $(docker ps -a -q)
```

Inspecting a container
```sh
$ docker inspect <container name>
```

Listing docker images locally available
```sh
$ docker images
```

Pulling an image from docker hub
```sh
$ docker pull <image>
```

Removing docker images
```sh
$ docker rmi <image name>
```

Removing ALL docker images
```sh
$ docker rmi -f $(docker images -q)
```

Go to interactive mode in a container
```sh
$ docker attach <idcontainer>
``` 

SSH connection to the container
```sh
$ docker exec -i -t <idcontainer> /bin/bash
```
