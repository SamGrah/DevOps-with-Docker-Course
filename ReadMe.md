This repo contains the Coursework exercises for DevOps with Docker course @ https://devopswithdocker.com/.<br /> This ReadMe contains the summary notes taken while progressing through the course.

# Docker with DevOps

## PART 1



### WHAT IS DOCKER?

---

###### Docker is a program for creating containers which contain include software (code) and its dependencies. Docker also offers tools which allow for communications between different containers.

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

#### *Images*

A docker image is a file from which containers are built. Docker images are functionally immutable because they cannot be modified after they are created. However, new **layers** can be added ontop an image. 

All docker images present on a local PC can be printed to the termial using the command `docker image ls`.
Docker images are built from an instruction file called a **Dockerfile** as a result of the terminal command `docker image build`.

###### Searching Images

Images can be searched on Docker Hub using the terminal command `docker search <title_text>`. The returned search results indicate which images are official. Where offical images are those that have been reviewed by Docker employees, are actively maintained, and have been built from repos in the docker-library.

###### Pulling Images

Images can be pulled from sources other than Docker Hub by specifying the address of the docker image in the pull command. For example, the `docker pull quay.io/norstrom/hello-world` command will download the image from `quay.io` rather than Docker Hub. 

###### Tagging Images

When specifying images, if a tag is not a specified, then the `latest` tag will be used for the pull command. The pull command then takes the form `docker pull image <image_name>:<tag>`. 
Tags as pseudonyms for other image tags locally. For example, as a result of the terminal command `docker tag ubuntu:18.04 ubuntu:bionic`, utilizing the `ubuntu:bionic` tag will act as a reference to the `ubuntu:18.04` tag.

Basic Dockerfiles contain each of the following commands: `FROM`, `RUN`, `CMD`. 


```dockerfile
FROM <image>:<tag>

RUN <install some dependencies>

CMD <command that is executed on `docker containter run`>
```

Each step of the Dockerfile is executed as a step by the docker deamon. Each step 'adds a layer' to the image. The layers act as a cache and can be skipped builds or rebuilds if there is no change to the contents of the cache. 

###### `FROM`

The `FROM` keyword is used to define the base image from which the Dockerfile builds an image.

###### `RUN`

When the image is created commands following the `RUN` keyword are executed. Docker `RUN` commands are automatically executed using the prefix `/bin/sh -c`. In addition to installing dependencies, the `RUN` command is also often used for establishing file permissions. 
Dockerfiles support multiple `RUN` statements but only a single `CMD` statement.

###### `CMD`

The `CMD` keyword is generally meant for invoking software (start a server) or specific code meant to be executed when the docker container is run.

###### `ENV`

Settings which trail the the `ENV` keyword are stored as enviornment variables within the image. The `export` command doesn't need to be included.

###### `COPY`

Files and folders specified after the `COPY` keyword will be copied into the working directory within the image.

###### `WORKDIR`

The folder structure which trails the `WORKDIR` keyword is created within the image and set as the working directory of the image.

###### `ENTRYPOINT`

Trailing arguments passed in the `docker container run` command are combined with the executable specified by the `ENTRYPOINT` keyword. `ENTRYPOINT` is less flexible than `CMD` because `ENTRYPOINT` must specify an executable command.

###### `EXPOSE`

The `EXPOSE` keyword is used to establish and map a container port to the host machine. 



#### *Container*s 

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

| Terminal Command                 | Description                                                  |
| -------------------------------- | ------------------------------------------------------------ |
| **IMAGES** ||
| `docker image pull <image_name>` | Download image without container creation                    |
| `docker image build`             | Create Image from Dockerfile                                 |
| `docker image rm <image_name>`   | Remove Image Locally                                         |
| `docker image prune`             | Remove All Dangling Images<br />*Dangling Images are those images without a name which aren't used* |
| `docker image ls`                | List Images                                                  |
| **CONTAINERS** |                                                              |
| `docker container run <image>` | Create Container from Image |
| `docker container run <container_name>` | Run Container |
| `docker container stop <container_name>` | Cease Container Execution |
| `docker container rm <container_name>` | Delete Container Locally |
| `docker container prune` | Remove All Stopped Containers |
| `docker container ls` | List Running Containers |
| `docker container ls -a |grep <matching_text>` | List All Containers |
| `docker container ls -a |grep <matching_text>` | Search All Containers with Title Containing Matching Text |
| `docker container exec <container_name>` | Executes a command inside the docker container |
| `docker pause <container_name>` | Pauses container execution |
| `docker unpause <container_name>` | Unpauses container execution |
|  |  |



### RUNNING AND STOPPING CONTAINERS

---

By defualt containers run using the `docker run <container_name>` command are dettached from the current terminal. The following flags can be used to allow terminal access to the container and allow terminal commands to be transmitted to the container of choice.

###### Container Run Flags

The following flags are applied to the `docker run <container_name>` command

| Flag                              | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| `-d`                              | Detached<br />Run container detached from current terminal shell |
| `-t`                              | Initiate shell prompt in current terminal session            |
| `-i`                              | Interactive<br />Allow container to recognize terminal commands |
| `--name <container_name>`         | Assign name to created iner                                  |
| `--rm`                            | Automatically delete garbage containers when the container is exited. Includes primary container. |
| `-v <host_path>:<container_path>` | Established a volume between host and container via the specified `host_path` and `container_path` locations. |
| `-p <host_port>:<container_port>` | Maps the exposes and maps container port to host port        |



###### Executing Commands After Container Startup

Any commands which follow the `container_name` in the `docker run` command are applied by the container itself after it has begun execution.
For example, due to the following command...
`docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'`
The container named `looper` will execute the shell script 

```shell
while true; 
do date;
sleep 1;
done
```



###### Logging Container Output

The currently logged output of a detached container can be show in the current local terminal by using the command `docker logs <container_name>`.
To continuously log the output of a container live, the terminal command `docker logs -f <container_name>` can be used.

###### Running Containers and Terminals

Containers can be attached to addtional terminal windows during runtime by invoking the terminal command `docker attach <container_name>` within the desired terminal.
However, if Ctrl+c is entered in either window, the container process stops execution (exits). This behavior can be avoided passing the `--no-stdin` flag when attaching the container.

###### Passing Keywords to Containers

If the command specified by `ENTRYPOINT` keyword takes an argument (like a script), then an argument can trail the container `run` command and be recognized by the script. Any trailing keywords are treated as arguments by the docker daemon.



### Volumes

---

A volume is simply a folder (or file) that is shared between the host machine and a container. Changes to the folder/file by a program running inside the container will alter the folder/file on the host machine and the file will be preserved even if the container exits.
Volumes can be defined by specifying the the container file/folder and host machine file/folder within the docker run command. The `-v` flag is used to specify volume defintion where the syntax is defined as `docker run -v <host_path>:<container_path> <container_name>`.



### Communication between Container & Host Machine

---

Communication between a host machine and running container can be accomplished via exposed ports. Within a Dockerfile the port specified by the `EXPOSE` keyword is configured to communicate outside the container (exposing a port).
Additionally, the `docker container run` command must specify a mapping between container port and host port. The docker run command takes the form `docker run -p <host_port>:<container_port> <container_name>`.



# PART 2

###### 

### Docker Compose

---

Docker compse is a tool used to mange and coordinate multiple containers. Using a `docker-compose.yml` file called a YAML file, multi-container applications can be run using a single command. The format of the compose YAML file:

```yaml
version: "3.9" #optional

services:
	web:
		build: .										# location of Dockerfile from which to build image
		ports:											# maps container exposed port to host machine port
			- "5000":"5000"
		volumes:										# defines file/folder mapping(s) for container --> host
			- .:/code
			- logvolume01:/var/log
		links:
			- redis
		redis:
			image: redis
	volumes:
		logvolume01: {}
```

The YAML file is stored in the project root in the same directory as the Dockerfile. The compose YAML file is composed of the commands and option flags that the docker command line utiliy uses to manipulate images, containers, volumens, repositories, etc. In effect, the docker compose YAML file allows for "automation" of container creation and/or orchestration between containers.



### Web Services using Docker Componse

---

The main purpose for Docker Compose is to run web services (HTTP). This can



