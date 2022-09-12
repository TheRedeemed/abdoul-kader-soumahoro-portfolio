---
title: "Intro to Containers and Docker"
description: "This article is an introduction to containers and to Docker."
date: "2022-09-01"
banner:
  src: "../../images/ian-taylor-jOqJbvo1P9g-unsplash.jpg"
  alt: "Container"
  caption: 'Photo by <u><a href="https://unsplash.com/photos/jOqJbvo1P9g">Ian Taylor</a></u>'
categories:
  - "Docker"
keywords:
  - "Virtual Machines"
  - "Containers"
  - "Docker"
---

# The Problem

In the past, applications were deployed on physical servers. They were many overheads related to slow application provisioning, hardware cost, maintnance, etc...  
**Virtual machines** were then introduced as a solution to these challenges. They provided multiple advantages like:
- Allowing multiple Operating Systems to be installed simultaneously on the same machine
- Faster application provisioning
- Easy maintnance etc...

However, virtual machines have some drawbacks. Each virtual machine relies on the resources of the physical host machine (machine on which the virtual machines are installed).
This means that we can have a limited number of virtual machines. One other major drawback is that each virtual machine need their own operating system and required libraries for applications to run.

# The Solution

**Containers** were introduced as an improvement on Virtual machines. Only one operating system is required on the host machine. All the containers share the **core/kernel** of that operating system and only run the libraries and software they need. 

Also, unlike virtual machines, containers do not required pre-allocated disk space and memory, each application within a container uses the resource they need. This helps optimize the utlization of resources.

Containers have the following benefits:
- More lightweight and therefore faster than VMs
- Easy to run a devops process to build, test and deploy applications faster
- Help create portable applications - same instance running locally and in the cloud or any other environment without any concerns about missing libraries
- More suitable for a microservice architecture - each microservice can be deployed in its own container.
- Easier blue/green deployment
- Easier scaling
- Self contained - each container can run their own version of software and libraries
- Multiple containers can run on the same host

# Docker

Docker is a software used to create containers. A container is normally used to run a single process or a small group of processes to provide a service.
It is built from an image. An image is created with a Dockerfile, a file containing sets of instructions to build a container.
Images are stored in an image registry such as **Dockerhub**. It is possible to have a private repository.

#### Dockerfile

Here is a sample Dockerfile

```Dockerfile
FROM debian:stable
LABEL author="Abdoul Soumahoro"

RUN apt update && \
	apt install -y nano && \
	apt clean
		
CMD ["/bin/bash"]
```

Docker isolation techniques such as cgroups(control groups) and namespaces:
- **Control Group** provide resource quota which limits one container from using resources like CPU, disk read/write to the detriment of other containers and the host itself.
- **Namespaces** are used to limit what each container can access on a host machine and in other containers.
	
A Docker image is built on top of a small base image. In the Dockerfile above, the base image is "debian:stable" (the stable version of debian).
Docker uses a layered filesystem model in images. Files required for additional libraries are saved in a new layer of their own and added on top the base image. 
This helps maintain the size of the container relatively small.

#### Basic Docker commands

Below are some basic docker commands:

- This command runs a debian container

```docker
docker run -dit debian
```

**-d** : detached <br>
**-it** : interactive 

- To check containers that are running we run the command below. This displays the container ID, the image, the time it was created, the status, the ports it running on and the user friendly name.

```docker
docker ps
```

- This will stop the container

```docker
docker stop <container_id>
```

- Lists of images downloded locally by Docker

```docker
docker images
```

- Display a containers detailed information

```docker
docker inspect <docker_id>
```

- This create an image from a container

```docker
docker commit <container_id><image_name> 
```

# Conclusion

In this article we learned about container as an improvement upon virtual machines. We went over the benefits of containers. We also learned Docker about which is very popular container software. We discussed how a container can be created with Docker and some basic Docker commands to run a container, stop a container, list images etc...