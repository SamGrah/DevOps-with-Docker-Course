This repo contains the Coursework exercises for DevOps with Docker course @ https://devopswithdocker.com/. 
The following summary notes were taken while progressing through the course.

# Docker with DevOps

## PART 1



### WHAT IS DOCKER?

---

Docker is a program for creating containers which contain include software (code) and its dependencies. Docker also offers tools which allow for communications between different containers.

![](https://docker-hy.github.io/images/1/container.png)

### WHAT ARE THE BENEFITS OF CONTAINERS?

---

##### *Works on My Machine* 

Often configuration of a development environment on a single PC requires esoteric settings management. This setting management can be difficult to recreate for other PCs because the setting are esoteric or the additional PC configurations vary.
Containters easily address this problem by allowing developers to share the same image or container.

##### *Isolated Enviornments*

Two different applications, using the same langauage or program, running in the same enviornment (local PC) can require two different versions of the same dependency (package, interpreter, compiler, etc). 
Containers allow for each application to execute within its own isolated container such that enviornment for each application contains the appropriate dependencies.

##### *Development*

Pre-defined containers or images drastically reduce the amount of time required for installation of complicated software programs/packages. For example, a database can easily be 'spun up' (installed and executing) by using only a few docker commands.

##### *Scaling*

Starting and stopping a docker container requires very little processor resources (time / bandwidth). Due to this, orchestration tools can easily create and run new copies of a docker container in order to satisfy large amounts of traffic for a site or service.



### RUNNING CONTAINTERS

---

The terminal command `docker container run <container_name>` instructs the docker deamon to begin execution of the container named `container_name`. If the container is defined on the local PC, then that container is spun up. 
Else, the docker deamon searches the Docker Hub online repository of images. The docker deamon downloads the image if it exists on Docker Hub, builds a container from the image, and executes the container.

```shell
$ docker container run hello-world
  
  Hello from Docker!
  ...
```



### IMAGE AND CONTAINERS

---

#### *Image*

A docker image is a file from which containers are built. Docker images are functionally immutable because they cannot be modified after they are created. However, new **layers** can be added ontop an image. 

All docker images present on a local PC can be printed to the termial using the command `docker image ls`.
Docker images are built from an instruction file called a **Dockerfile** as a result of the terminal command `docker image build`.

Basic Dockerfiles contain each of the following commands: `FROM`, `RUN`, `CMD`

```dockerfile
FROM <image>:<tag>

RUN <install some dependencies>

CMD <command that is executed on `docker containter run`>
```



#### *Container* 

Containers only contain the code and dependencies necessary to execute an application. They are *isolated* enviornment which can communicate both with the host machine and other containers using defined ports. From the perspective of the host PC, containers can be thought of as processes which can communicate via defined ports.

Docker containers can be created from existing images using the `docker container run <image>` terminal command.

All containers can be listed to the terminal using the command `docker container ls`. The `-a` flag can be used to display only running containers.

```shell
$ docker container ls -a
  CONTAINER ID   IMAGE           COMMAND      CREATED          STATUS                      PORTS     NAMES
  b7a53260b513   hello-world     "/hello"     5 minutes ago    Exited (0) 5 minutes ago              brave_bhabha
  1cd4cb01482d   hello-world     "/hello"     8 minutes ago    Exited (0) 8 minutes ago              vibrant_bell
```



### DOCKER CLI BASICS

---

The 'Docker Enginer' contains 3 parts; 

- CLI
- REST API (running locally)
- docker deamon

When commands are run, the docker client transmits the command through the REST API to the docker deamon. The docker deamon manages the all images, containers, and docker resources. 

###### Create Image from Dockerfile 

`docker image build`

###### Remove Image Locally

`docker image rm <image_name>`

###### Remove All Dangling Images

<!--Dangling Images are those images without an assoicated container-->
`docker image prune`

###### List Images

`docker image ls` 

###### Create Container from Image

`docker container run <image>`

###### Delete Container Locally

`docker container rm <container_name | container_id>`

###### Remove All Stopped Containers

`docker container prune`

###### List Containers

Running Containers 				`docker container ls`
All Containers  						`docker container ls -a | grep <matching_text>`
Search Matching Containers	`docker container ls -a | grep <matching_text>`

###### 



