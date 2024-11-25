# Docker-PGV
# Ruben Abreu Gonzalez

## Indice
- [Docker-PGV](#docker-pgv)
- [Ruben Abreu Gonzalez](#ruben-abreu-gonzalez)
  - [Indice](#indice)
    - [Paso 1](#paso-1)
    - [Paso 2](#paso-2)

### Paso 1
> [!TIP] sudo docker run hello-world
```bash
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete 
Digest: sha256:305243c734571da2d100c8c8b3c3167a098cab6049c9a5b066b6021a60fcb966
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### Paso 2
> [!TIP] 
> sudo docker ps
```bash
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
---
> [!TIP] 
> docker ps -a
```bash
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
65ad0ce82e23   hello-world   "/hello"   10 minutes ago   Exited (0) 10 minutes ago             strange_banach
```
---
>[!TIP] 
> sudo docker ps -l
```bash
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
65ad0ce82e23   hello-world   "/hello"   15 minutes ago   Exited (0) 14 minutes ago             strange_banach
```
---
> [!TIP] 
> sudo docker images
```bash
REPOSITORY               TAG                 IMAGE ID       CREATED         SIZE
hello-world              latest              d2c94e258dcb   19 months ago   13.3kB
```