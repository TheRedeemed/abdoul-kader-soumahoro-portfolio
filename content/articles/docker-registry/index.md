---
title: "Docker Registry and Images"
description: "This article is a primer on docker registry and images"
date: "2022-09-11"
banner:
  src: "../../images/chuttersnap-fN603qcEA7g-unsplash.jpg"
  alt: "Container"
  caption: 'Photo by <u><a href="https://unsplash.com/photos/fN603qcEA7g">CHUTTERSNAP</a></u>'
categories:
  - "Docker"
keywords:
  - "Dockerhub"
  - "Dockerfile"
  - "Registry"
  - "Repository"
  - "Images"
---


# Registries

Docker Hub is the default image registry. The primary function of a registry is to store images. We do not only pull images from a registry, we can also push an image to a registry.

A repository within a registry is a collection of one or more images. Each image is stored as a tag. A tag normally refers to a version of an image but it can also refer to a variation of an image.

For example, we may have a couple of mysql images based one using alpine and the other debian as base image. In this scenario, the alpine and debian can be used as tags for each images.

Let's pull the **ubuntu:bionic** image from docker hub with the following command:

```docker
docker pull docker.io/ubuntu:bionic
```

<u>Note:</u> The command above is the same as **docker pull ubuntu:bionic**.

Let perform another pull

```docker
docker pull registry.hub.docker.com/library/ubuntu:bionic
```

The command we pull another ubuntu image with the same tag but from a different dns name - registry.hub.com

Let's check the local images with "docker images". 

```
user@node:~$ docker images
REPOSITORY                               TAG       IMAGE ID       CREATED      SIZE
ubuntu                                   bionic    35b3f4f76a24   4 days ago   63.1MB
registry.hub.docker.com/library/ubuntu   bionic    35b3f4f76a24   4 days ago   63.1MB
```

We can see that the images have the same image ID and tag. However their repository values are different. 

In order to start a container from the image pulled from the hub.docker.com DNS we will have to use the following command:

```docker
docker run -dit registry.hub.docker.com/library/ubuntu:bionic
```

This shows that the docker images are built in this format: 
<registry address>/<repository>/<image>:<tag>

The repository therefore influences the name of the image.  
As we can see, the image downloaded from docker.io doesn't display the repository. This is due to how docker hub handles official images - usually no repository is shown.

When pulling an unofficial image, we need to use the <namespace>/<repository>

For instance we can use the following command to pul the unofficial **mysql** image.

```docker
docker pull mysql/mysql-server
```

Besides Docker Hub there are other container registry like: 
- Amazon's Elastic Container Registry (ECR),
- Quay from RedHat, 
- Google's container registry. 

It is also possible to have a private registry to store images that contain sensitive information.

The **docker login** command is used to access a private registry.

<br>

# Images with Dockerfiles

#### Dockerfile instructions

**FROM:** Sets the base image - The base of the image being created<br>
**CMD:** Sets the command to be executed when running the container created from the image. It can also be used to set the default argument for the **ENTRYPOINT** instruction.<br> 
**RUN:** Execute a command to help build the image<br>
**EXPOSE:** Opens networking ports<br>
**VOLUME:** Sets a disk share<br>
**COPY:** Copies files from the local disk into the container built from the image being created<br>
**LABEL:** Adds metadata to an image in a key-value pair. Like MAINTAINER = "maintainer name"<br>
**ENV:** Sets environment variable<br>
**ENTRYPOINT:** Specified at the end of the Dockerfile. It determines which executable runs when the container starts. (It can use CMD to pass an argument to the executable)<br>

Let's look at this Dockerfile:

```
FROM debian:latest
LABEL maintainer="Abdoul Soumahoro"
ENTRYPOINT ["/bin/ping"]
CMD ["www.docker.com"]
```

The image built based on this Dockerfile will have "debian" as the base image. The LABEL show is the maintainer of the Dockerfile.
The **ENTRYPOINT** command indicates that the **"/bin/ping"** executable will run once the container start. This ping will run on **"www.docker.com"**. 

In order to build an image with this Dockerfile, we will use the following command:

```docker
docker build -t abdouls/dockerping:latest .
```

The command is using this pattern

docker build -t <namespace>/<repository>:<tag> <location of Dockerfile>

<u>Note:</u> In the command above, the dot (.) at the end of the command indicates that the Dockerfile is in the current folder.

In order to push the image to a registry we will login with the **"docker login"** command. 

Once logged in, we can push the image with the command below:

```docker
docker push abdouls/dockerping:latest
```

We can run a container with the abdouls/dockerping image.

```docker
docker run abdouls/dockerping:latest
```

When we run the container we can see that as soon as the container starts, it is pinging docker **"www.docker.com"**.

#### Override entrypoint

**"www.docker.com"** is the default value provided in the Dockerfile with the **CMD** command. 
It is possible to override this argument by passing the value we would like. 
If for instance, instead of pinging docker.com, we would like to ping "google.com", we run the container with the command below:

```docker
docker run abdouls/dockerping:latest google.can
```

As we can see it now pinging google.com instead.


In case we need a command that is not available by default in our base image then we would need to add that command by installing the package for that command.

Let's look at the Dockerfile below:

```
FROM debian:latest
LABEL maintainer="Abdoul Soumahoro"
RUN apt update && \
	apt install -y traceroute && \
	apt install -y curl && \
	apt clean
ENTRYPOINT ["/bin/bash"]
```

<u>Note:</u> 
- The "&&" is a way to concatenate command. When they added, they will ensure the previous command executes successfully before running the following command.
- The "\" is to add a line continuation character. It indicates that the line is not done. It helps make a series a commands more readable
	
Let's build an image with this Dockerfile

```docker
docker build -t abdouls/nettools .
```

Once the images is built, let's run a container using that image

```docker
docker run -it abdouls/nettools
```

Since the entrypoint of the contain is **"bin/bash"**, we can see that the container starts with the bash prompt. We can run a curl and traceroute.

```
curl google.com
traceroute google.com
```

See the result below:

```
user@node:~$ docker images
REPOSITORY                               TAG       IMAGE ID       CREATED          SIZE
abdouls/nettools                         latest    6fd9a4590614   35 seconds ago   150MB
registry.hub.docker.com/library/ubuntu   bionic    35b3f4f76a24   4 days ago       63.1MB
ubuntu                                   bionic    35b3f4f76a24   4 days ago       63.1MB
abdouls/dockerping                       latest    b03accb303e3   2 weeks ago      124MB

user@node:~$ docker run -it abdouls/nettools
root@42adc025b26f:/# curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
root@42adc025b26f:/# traceroute google.com
traceroute to google.com (142.250.217.238), 30 hops max, 60 byte packets
 1  172.17.0.1 (172.17.0.1)  0.615 ms  0.578 ms  0.555 ms
 2  * * *
 3  * * *
```

Since the container is working as expected, we can go ahead and push it to dockerhub

```docker
docker push abdouls/nettools
```

One thing to note is that in the Dockerfile for this image, we have an **ENTRYPOINT** but we didn't provide a **CMD**. 

Currently the entrypoint is **/bin/bash**. Let's assume we wanted override the entrypoint we could pass the **"--entrypoint"** flag along with the argument for the new entrypoint.

If we want replace the entrypoint with a "curl" command to "google.com" then we would do the following:

```docker
docker run --entrypoint /usr/bin/curl -it abdouls/nettools google.com
```

As we can see in the output, a curl call is made to google.com

```
user@node:~$ docker run --entrypoint /usr/bin/curl -it abdouls/nettools google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```
  
However, a better way to override an entrypoint would be to use the CMD. Let's look at the Dockerfile below.

```
FROM debian:latest
LABEL maintainer="Abdoul Soumahoro"
RUN apt update && \
	apt install -y traceroute && \
	apt install -y curl && \
	apt clean
CMD ["/bin/bash"]
```

We wil run the following commands to build the image and run the container:

```docker
docker build -t abdouls/nettools:allow-override
docker run -it abdouls/nettools:allow-override
```

The container starts with a /bin/bash prompt.

```
user@node:~$ docker run -it abdouls/nettools:allow-override
root@b333161056f1:/#
```

Let's say instead of starting with a bash prompt, we want the container to start a curl, we would do the following:

```
docker run -it abdouls/nettools:allow-override curl google.com
```

As we can see, the container starts with a curl instead of starting with a bash prompt.

```
user@node:~$ docker run -it abdouls/nettools:allow-override curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```

we can push our new image with the following command:

```docker
docker push abdouls/nettools:allow-override
```

<br>

# Conclusion

In This article, we learned where images are hosted and the naming convention used to access those images. We also learned the most common instructions to Dockerfile. We also learned how to build an image and how to push it to dockerhub.