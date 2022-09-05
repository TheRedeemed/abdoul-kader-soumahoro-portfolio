---
title: "Managing docker images"
description: "This article covers how to download an image, view the history of an image, tag an image, view and clean up storage for images"
date: "2022-09-05"
banner:
  src: "../../images/guillaume-bolduc-uBe2mknURG4-unsplash.jpg"
  alt: "Container"
  caption: 'Photo by <u><a href="https://unsplash.com/photos/uBe2mknURG4">Guillaume Bolduc</a></u>'
categories:
  - "Docker"
keywords:
  - "Containers"
  - "Image"
  - "Tag"
---

# Understand layering model in docker

Each changes made to an image is saved into a layer in such a way if that layer is required for another container, only the additional changes for that other container are saved.

Suppose we already have a debian image and we are creating another image that will contain debian as a base image in addition to some other changes. 
Since we already have the debian image, only the additional changes needed for that other container will be downloaded. The Debian image will not be pull again. This model helps save disk space.


# Download image

The **docker pull** command is used to pull an image. It takes an image name as an argument and downloads that image to the host.

The command below pulls the **nginx** image

```docker
docker pull nginx
```

Once an image is downloaded, we can check it's presence with the docker images command with the command below:

```docker
docker images
```

Below is the output of the docker images command

```
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
nginx        latest    2b7d6430f78d   13 days ago     142MB
```

# View history of an image

The docker history command shows us how an image was constructed.

The command below shows the history of the nginx image:

```docker
docker history nginx
``` 

Below is the output:

```
C:\WINDOWS\system32>docker history nginx
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
2b7d6430f78d   13 days ago   /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B
<missing>      13 days ago   /bin/sh -c #(nop)  STOPSIGNAL SIGQUIT           0B
<missing>      13 days ago   /bin/sh -c #(nop)  EXPOSE 80                    0B
<missing>      13 days ago   /bin/sh -c #(nop)  ENTRYPOINT ["/docker-entr…   0B
<missing>      13 days ago   /bin/sh -c #(nop) COPY file:09a214a3e07c919a…   4.61kB
<missing>      13 days ago   /bin/sh -c #(nop) COPY file:0fd5fca330dcd6a7…   1.04kB
<missing>      13 days ago   /bin/sh -c #(nop) COPY file:0b866ff3fc1ef5b0…   1.96kB
<missing>      13 days ago   /bin/sh -c #(nop) COPY file:65504f71f5855ca0…   1.2kB
<missing>      13 days ago   /bin/sh -c set -x     && addgroup --system -…   61.1MB
<missing>      13 days ago   /bin/sh -c #(nop)  ENV PKG_RELEASE=1~bullseye   0B
<missing>      13 days ago   /bin/sh -c #(nop)  ENV NJS_VERSION=0.7.6        0B
<missing>      13 days ago   /bin/sh -c #(nop)  ENV NGINX_VERSION=1.23.1     0B
<missing>      13 days ago   /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B
<missing>      13 days ago   /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>      13 days ago   /bin/sh -c #(nop) ADD file:7726efb0e0eb5003d…   80.4MB
```

The output shows each layer line by line along with the abbreviated command used to create that layer. In this case, some of the hashes are missing because the image was not constructed on the current host.

# Tag an image

When viewing an image, the **"tag"** column displays the version of an image.

A tag can be added to an image in order to specify the version of that image. We can see the usage of the tag command with docker help.

```docker
docker tag --help
```

It displays the following output

```
Usage:  docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
```

<u>Note:</u> When looking at the help, everything in bracket is optional "[]". In the ouput above the **[:TAG]** is optional. 
If it is not specified then the image is tagged with **"latest"**.

Currently the nginx image is tagged with latest. Let's use the command below to tag that image with **"my_blog_stable"** 

```docker
docker tag nginx:latest nginx:my_blog_stable
```

Here is the result of:

```
C:\WINDOWS\system32>docker images
REPOSITORY   TAG              IMAGE ID       CREATED         SIZE
nginx        latest           2b7d6430f78d   13 days ago     142MB
nginx        my_blog_stable   2b7d6430f78d   13 days ago     142MB
```
We see that both images have the same image ID. This is because, instead of duplicating the **nginx:latest** image and use up space, docker simply creates an alias for the my_blog_stable image. This alis is used to the original image.
It is important to note that deleting the original image <u>will</u> not delete the **"my_blog_stable"** image.

We can view tags available for images at <u>hub.docker.com</u>. 

Let's look at the dockerfile below: 

```docker
FROM debian:buster-slim
LABEL maintainer="NGINX Docker Maintainers <docker-maint@nginx.com>"

ENV NGINX_VERSION	1.17.2
ENV NJS_VERSION		0.3.3
ENV PKG_RELEASE		1~buster

RUN apt-get update \
	&& apt-get install -y nginx \
	&& apt-get clean
	
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/lof/nginx/error.log

COPY index.html /var/www/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]	
```

**FROM** tells docker what to use as base image. Here the **buster-slim** version of debian will be used as the base image.

**LABEL** indicates the contact information of the author of the dockerfile.

**ENV** declares environment variables.

**RUN** indicates shell commands to run in order to help build the image. 

- Concatenating the commands with the **"&&"** prevents docker from adding one layer for each command. It adds all the commands in the same layer
		
- While the package manager is installing packages, it also keeps a copy of the files downloaded to install those packages. After the packages are installed, there's no need to keep the files that were downloaded. Therefore, the **"apt-get clean"** command is used to remove all those files that are not needed anymore.
			 
**COPY** copies a file from the host into the image at build time. This is commonly used to add small config files into an image. Here the **index.html** file will be copied to the **/var/www/html/** folder.

**EXPOSE 80** will expose the port 80 in the container

**CMD** runs the given commands. 
- Here, it will run the nginx binary from within the container. 
- The "daemon off" argument keeps nginx running in the foreground as a container. Without this option, the container would stop running as soon as it started.

We can use the command below to create an image based on this dockerfile with a tag "mynginx"

```docker
docker build -t mynginx .
```

After the image is created, we can run the container with the following command

```docker
docker run -dit mynginx
```

To see the layers of this image, we can use the following command

```docker
docker history mynginx
```

# View and clean up storage used by docker for images

The **"docker images"** command can display all the images currently in our system.

```
C:\WINDOWS\system32>docker images
REPOSITORY   TAG              IMAGE ID       CREATED         SIZE
mynginx      latest           345d16cb3a3f   2 minutes ago   148MB
nginx        latest           2b7d6430f78d   13 days ago     142MB
nginx        my_blog_stable   2b7d6430f78d   13 days ago     142MB
```

<br>
In order to remove an image we will use the following command :

```docker
C:\WINDOWS\system32>docker rmi nginx:my_blog_stable
Untagged: nginx:my_blog_stable
```

We can see that in this case, the image was untagged but not deleted. That is because, this images was an alias or pointer to the **nginx:latest** image.

Let's see the result of docker rmi nginx:latest

```
C:\WINDOWS\system32>docker rmi nginx:latest
Untagged: nginx:latest
Untagged: nginx@sha256:b95a99feebf7797479e0c5eb5ec0bdfa5d9f504bc94da550c2f58e839ea6914f
Deleted: sha256:2b7d6430f78d432f89109b29d88d4c36c868cdbf15dc31d2132ceaa02b993763
Deleted: sha256:83cfe7b2ae78dff709b878ea6827c2b7447910f22f5c18d2155bc4ee678b6589
Deleted: sha256:8354a3628193232756e7e39423937a89bdea9738fcd033e3009c34847d44f1cc
Deleted: sha256:edd004a0175251a1fa027a4949079439eb989b9c9d0dc863c2c834e0da69ed2e
Deleted: sha256:e65b26350d575233ec4815a0ced147030bb9db419084e6a1561aab8c9f05f20e
Deleted: sha256:d75805fc21788b744c171b7ca303a153b34b61b61401c74348b0a16c9bfabbc8
Deleted: sha256:6485bed636274e42b47028c43ad5f9c036dd7cf2b40194bd556ddad2a98eea63
```

We can see that some images were untagged and some layers were deleted. This frees up some disk space.

Let's see the list images currently present:

```
C:\WINDOWS\system32>docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
mynginx      latest    345d16cb3a3f   8 minutes ago   148MB
```

<br>
Sometimes they are layers that are not used by any images. These unused layers are called **"dangling images"**

In order to remove dangling images we can use the docker prune command seen below:

```docker
docker image prune
```

To remove both unused images and dangling images, we can use this command:

```docker
docker system prune -a
```

The **"docker system df"** command show how much disk space is used by Docker.


# Conclusion

In this article We learned about the layering system used by Docker in order to save disk space.
We also learned how to download images, look at the history of an image, how to tag our and how to delete them.