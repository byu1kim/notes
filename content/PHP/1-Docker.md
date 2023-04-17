+++
title = "Docker"
menuTitle = "Docker"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Install

https://www.docker.com/get-started/

#### Docker VSC Extension

https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker

{{% notice info %}}
Window user needs WSL, Ubuntu
{{% /notice %}}

#### Install WSL

- Contianers, Hyper-V, Windows Subsystem for Linux Features enabled.
- Control pannel > uninstall program > turn windows features on/off
- Check Windows Subsstem for Linux
- Reboot

#### WSL Setting in VSC

- Adding WSL in VSC : Ctrl+, > wsl > check integrated: use wsl profiles

#### Install Ubuntu

- Ubuntu for WSL : Download Ubuntu 22.04.1 LTS from Microsoft apps
- PowerShell $ wsl --install
- Verify : $ wsl -l -v
- Version set : $ wsl --set-default-version 2
- VSC Extensions : WSL, Remote Development
- Docker : Settings > General > WSL2 based engine | Resources > Ubuntu-22.04 check

## Docker

Docker is platform for building, running, and shipping applications in a self contained environments - called containers.

Each of the containers are isolated, configured, environments - and by using the same container across development teams docker helps eliminate the otherwise inevitable "It works on my machine" issues that are otherwise common headaches for developers.

Another way to solve the same problem would be through the use of virtual machines. If everyone involved in a project sets up a a virtual machine (VMWare, VirtualBox, etc.) with the same specs you could also achieve the desired isolation and standardization, but each virtual machine requires a full operating system in order to run which can be costly, slow, and resource intensive.

`Containers`, on the other hand, are lightweight and much more performant because they at a low level they rely on the operating system of the host machine.

## Getting Started

Clone a repository

```
docker run --name repo alpine/git clone https://github.com/docker/getting-started.git
docker cp repo:/git/getting-started/ .
cd [repository name]
```

Build the image

```
cd getting-started
docker build -t docker101tutorial .
```

Run Container

```
docker run -d -p 80:80 \ --name docker-tutorial docker101tutorial
```

You should now be able to open a browser : http://localhost/tutorial

## Docker Images

Docker Images are, essentially, the blueprint for Docker Containers (which are essentially an image in its running state).

Images are made up with

- The runtime environment : Specific OS kernel, Core platform (Node, PHP...)
- Application code
- Dependencies that the application requires
- Configuration settings : Env, Port mappings
- Commands that need to run at build time
- The Image's Filesystem

{{% notice note %}}
Images are READ ONLY. When we want to make changes to an image, we build a new image (though we can, to an extent, use volume mappings to make them a bit more dynamic during development).
{{% /notice %}}

#### Parent Image

The first layer in an Image is usually a parent image, which supplies the the OS and runtime environment.

https://hub.docker.com/search?q=&type=image

Download Parent image :

```
docker pull node:10.24.1-alpine3.11 //name of parent image from dockerhub
```

## Docker Containers

Containers are, essentially, an isolated, runnable, instance of a Image.

Start Container : Run below in Terminal

```
docker run <paste your Image ID>
```

Run Interactive mode (keeping it from exiting immediately)

```
docker run -i <Image ID>
```

## Docker File (Dockerize)

### Simple Express app

```
npm init -y
```

And then create app.js

Create file in the root, name it `Dockerfile` (Capital D) and add below

```
FROM node:18-alpine

COPY . .

CMD ["node", "app"]
```

In terminal,

```
docker build -t nodeinabox .
```

### HTTP request app (More complex)

Folder structure

![Folder structure](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_28a1a99eab9647f281ff0afb9d894fd3_image.png?_&d2lSessionVal=rdw5g9rgU1OpW6RWwFGhZDCHG&ou=868291)

Dockerfile

```
# Specify our base image
FROM node:18-alpine

# Copy our files to the image
COPY . .

# Set the working directory
WORKDIR /server

# Install Project Dependencies
RUN npm install

# Expose Port 4000 for mapping
EXPOSE 4000

# Run our Express server
CMD ["node", "server"]
```

.dockerignore

```
node_modules
*.md
.git
```

Build in your Terminal

```
docker build -t express-in-a-box .
```

## Running a Container from the Image

Open Docker Desktop

Select the Images panel

Click the Run icon

Expand the Optional Settings

- Ports : 4000 and Run

## Docker Images, Layers, Caching

Docker Images are read only.

When we make changes to our source code we will need to rebuild our Image (until we discuss Volumes).

Though our simple Express app is doesn't have much by way of dependencies, it would be a royal pain if every time we wanted to build an Image for a React app it had to go out and download all of the node modules with each build.

Fortunately, Docker caches Layers and does its best to expedite things.

The thing to know here is that each Layer builds upon the previous and as soon as a Layer has changed all subsequent Layers cannot be constructed from cache.

#### Optimized Docker file

```
# Specify our base image
FROM node:18-alpine

# Set the working directory
WORKDIR /server

# Copy package.json to the image
COPY ./server/package.json .

# Install Project Dependencies
RUN npm install

# Copy the rest of our code.
COPY ./server .

# Expose Port 4000 for mapping
EXPOSE 4000

# Run our Express server
CMD ["node", "server"]
```

And build

```
docker build -t express-in-a-box .
```

and then build again

All our layers are CACHED! Nice!

Make a change in server.js and then build once more.

Since our changes happened in a Layer after the npm install, we successfully avoided re-installing dependencies in our build. Yay us!

## Docker Volumes

As developers we want to make development as quick and painless as possible so that we can have more fun. Even with caching in place, it's would be a pain to have to rebuild our container every time we make a change to our code.

Docker Volumes relieve this pain by allowing us map local directories to directories in our container.

Volumes come into play when we run a Container, without our Image having to be rebuilt until/unless we want to share our ship our app.

To demonstrate how this works, let's start by installing nodemon as a global package in our image, and then change the last CMD to run the dev script.

#### Dockerfile

```
# Specify our base image
FROM node:18-alpine
# Install nodemon, globally
RUN npm install -g nodemon
```

...etc

```
# Run our Express server via nodemon in the dev script
CMD ["npm", "run", "dev"]
```

#### package.json

```
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"dev": "nodemon --legacy-watch server.js"
},
```

{{% notice note %}}
The --legacy-watch flag is important. Without it nodemon won't detect changes in our container volume.
{{% /notice %}}

build

```
docker build -t express-in-a-box .
```

Jump back into Docker Desktop and run the image to spin up the container, but this time in the Optional Settings we'll also set up Volumes.
The Host path needs to be an absolute path to our app directory.
In my case it will be: C:\Users\jsolomon11\JBS\Courses\SSD\Intake28\WebDev2\Lectures\Day12\Docker\SSD-FWD2-DockerExplorer\ExpressInABox\
The Container path will just be an absolute path from the container root
/app
We also need to add a second volume in our container, this time without a Host path, so that it uses the node_modules from the container instead of our local.

## Docker Compose

We've explored plenty of Docker for one morning, but there's one more thing worth introducing before we set sail: Docker Compose.

Docker Compose is a tool for managing multiple Images and Containers that work together for a project.

For example, for a typical web application we might want to containerize:

- A web server / API back end.
- A database server
- A client-side app

To do so we would set up each of these with individual Dockerfile s, and in a parent directory we'd have a single docker-compose.yaml defining:

- The path to each Dockerfile
- The corresponding Container names
- Port mappings
- Volume Mappings

## Practice

The following is NOT required, but if you would like some practice working with Docker and/or you would like to be able to exclude the --no-experimental-fetch flag when running your Code Challenge project... See if you can figure out how to run it in a Node v16 container.

```
Find an appropriate parent image on Dockerhub
Go to Dockerhub
Search Node
Switch to the Tags tab
Filter with "16-a"
Pull the desired parent image
docker pull node:16-alpine
Add a Dockerfile in the root of the project
Specify the base image
FROM node:16-alpine
Set a working directory for volume mapping
WORKDIR /app
Copy package.json to image
COPY package.json .
Install project dependencies
RUN npm install
Copy the rest of your files
COPY . .
Expose port 3000
EXPOSE 3000
Run gulp
CMD ["npx", "gulp"]
Build the image
docker build -t code-challenge:node16 .
Open Docker Desktop
Locate the image
Click Run
Under Optional Settings
Set the Host port to 3000
Under Volumes
Set Host Path to the absolute path to your project root i.e. C:\Users\jsolomon11\JBS\Courses\SSD\Intake28\WebDev2\Lectures\Day12\Docker\SSD-FWD2-DockerExplorer\html-project-master

Set Container path to /app
Run
Open your browser to localhost:3000
```

That's it. The project is now running in a container with Node v16
