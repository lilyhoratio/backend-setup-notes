# Docker

## Bundle app into an image

- `docker pull node:latest`
- In a Dockerfile:

  ```docker
  # What image do you want to start building on?
  FROM node:latest

  # Make a folder in your image where your app's source code can live
  RUN mkdir -p /src/app

  # Tell your container where your app's source code will live
  WORKDIR /src/app

  # What source code do you what to copy, and where to put it?
  COPY . /src/app

  # Does your app have any dependencies that should be installed?
  RUN yarn install

  # What port will the container talk to the outside world with once created?
  EXPOSE 3000

  # How do you start your app?
  CMD ["npm", "start"]
  ```

- `docker build -t nodeserver .` : from location of Dockerfile you want to build (make sure to be in that folder)
  - `docker build`: takes Dockerfile and creates an image from steps detailed
  - `-t`: tag onto image to identify it later
  - `nodeserver`: name tagged onto image
  - `.` relative path to Dockerfile that we want to build onto an image

## Container lifecycle

- Spin up a container:

  - `docker run --name <container-name> <image-name>` (expects name of image that is cached (saved) locally on machine)
  - `docker run -d -p 80:80 -- rm --name webserver nginx`
    - `nginx`: name of image to spin into container
    - `--name webserver`: (optional) unique name to container
    - `-p 80:80`: connect port 80 on host to port 80 on container. Image has exposed port ready to broadcast and receive data.
    - `-d`: indicator to run container in a detached state (in the background)
    - `-rm`: automatically deletes itself when stopped, so you don't need to do `docker rm <container-name>`

- `docker stop webserver`: stops the container (if created with `-rm`, then will also delete the containers)
- `docker start webserver`: restarts
- `docker rm -f webserver`: deletes container from machine. `-f` flag (optional) forces container to stop and then removes it.
- `docker rmi <image-name>`: clear out nginx image
- When removing images, remember to remove containers associated with them.

## Housekeeping

- `docker pull posrgres` or `docker pull postgres:[tag_you_want]`

- `docker info`

  ```
  Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
  Images: 1
  ```

- `docker images -a`: lists all images. `-a` shows untagged images that come with building other images.

- `docker ps -a`: lists containers. `- a` flag lists containers that are running on your machine.

- `docker logs <container-name>`: revelas everything that has been logged inside the container so far. Cool!

## Containerized Development with Volumes

> Volume is a free-floating file system. Allows a container to interact with a host's filesystem.

```
[nodemon] restarting due to changes...
[nodemon] starting `node index index.js`
listening on port 8080...
```

- `-v` to mount a volume, expects two arguments: path to directory you want the spun-up container to reference, and path to directory inside the container where you want file changes to be reflected
  - `docker run -d -p 1000:8080 -v $(pwd):/src/app colorserver`
