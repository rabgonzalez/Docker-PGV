# Docker-PGV
# Ruben Abreu Gonzalez

## Indice
- [Docker-PGV](#docker-pgv)
- [Ruben Abreu Gonzalez](#ruben-abreu-gonzalez)
  - [Indice](#indice)
  - [tarea 1](#tarea-1)
    - [Paso 1](#paso-1)
    - [Paso 2](#paso-2)
  - [Tarea 2](#tarea-2)
    - [Paso 1](#paso-1-1)
    - [Paso 2](#paso-2-1)
    - [Paso 3](#paso-3)
    - [Paso 4](#paso-4)
    - [Paso 5](#paso-5)
    - [Reto Adicional](#reto-adicional)
      - [Cambiar de puerto](#cambiar-de-puerto)
      - [Crear un volumen](#crear-un-volumen)
  - [Tarea 3](#tarea-3)

## tarea 1
### Paso 1
> [!TIP] 
> sudo docker run hello-world

Ejecutamos una imagen de Docker, como no existe, la descarga con **docker pull hello-world**
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

Muestra los contenedores activos (corriendo)
```bash
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
---
> [!TIP] 
> docker ps -a

Muestra los contenedores
```bash
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
65ad0ce82e23   hello-world   "/hello"   10 minutes ago   Exited (0) 10 minutes ago             strange_banach
```
---
>[!TIP] 
> sudo docker ps -l

Último contenedor creado
```bash
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
65ad0ce82e23   hello-world   "/hello"   15 minutes ago   Exited (0) 14 minutes ago             strange_banach
```
---
> [!TIP] 
> sudo docker images

Muestra las imagénes almacenadas en nuestro equipo
```bash
REPOSITORY               TAG                 IMAGE ID       CREATED         SIZE
hello-world              latest              d2c94e258dcb   19 months ago   13.3kB
```

## Tarea 2
### Paso 1
> [!TIP]
> docker --version

Muestra la versión de Docker
```bash
Docker version 27.2.0, build 3ab4256
```

### Paso 2
> [!TIP]
> docker pull tomcat

Nos descargamos la imagen de Docker en nuestro equipo
```bash
Using default tag: latest
latest: Pulling from library/tomcat
afad30e59d72: Already exists 
918d361e6529: Already exists 
cf4dd2a7e40b: Already exists 
d152af7a0148: Already exists 
a5d7958ebd69: Already exists 
409b41a58ed4: Pull complete 
4f4fb700ef54: Pull complete 
9eabaa92f491: Pull complete 
Digest: sha256:2ade2b0a424a446601688adc36c4dc568dfe5198f75c99c93352c412186ba3c9
Status: Downloaded newer image for tomcat:latest
docker.io/library/tomcat:latest
```
---
> [!TIP]
> docker images
```bash
tomcat                   latest              f77539e7e45f   2 weeks ago     467MB
```

### Paso 3
> [!TIP]
> docker run -d -p 9090:8080 --name tomcat-server tomcat

Ejecuta el programa de docker
- -d: lo ejecuta en segundo plano, dejando libre la terminal
- -p: declaramos el puerto que va a utilizar
- --name: le asignamos un nombre identificativo para manipularlo mejor 
```bash
bf062fdb3126df9d530ebc01a87b956d83e6782a91c99e833bd76146a91d46dd
```
---
> [!TIP]
> docker ps
```bash
CONTAINER ID   IMAGE     COMMAND             CREATED          STATUS          PORTS                                         NAMES
bf062fdb3126   tomcat    "catalina.sh run"   24 seconds ago   Up 23 seconds   0.0.0.0:9090->8080/tcp, [::]:9090->8080/tcp   tomcat-server
```

### Paso 4
> [!TIP]
> http://localhost:9090

Vamos a la página en la que se está ejecutando el programa
```bash
Estado HTTP 404 – No encontrado
```
---
> [!TIP]
> docker logs tomcat-server

Muestra un seguimiento del contenedor (errores, avisos, peticiones...)
```bash
25-Nov-2024 23:14:50.899 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [162] milliseconds
```
---
> [!TIP]
> lsof -i :9090

Nos informa si el puesto ya está siendo utilizado por otro programa

### Paso 5
> [!TIP]
> docker stop tomcat-server

Paramos el contenedor
```bash
tomcat server
```
---
> [!TIP]
> docker rm tomcat-server

Eliminamos el contenedor de Docker
```bash
tomcat server
```
---
> [!TIP]
> docker rmi tomcat

Eliminamos la imagen de nuestro ordenador
```bash
Untagged: tomcat:latest
Untagged: tomcat@sha256:2ade2b0a424a446601688adc36c4dc568dfe5198f75c99c93352c412186ba3c9
Deleted: sha256:f77539e7e45f7c6337c681589fe18ee6407640e4066c450fcfb8c6a4ba5575b2
Deleted: sha256:ec216f70111f7e403bf2dd00786888a33e31e0b7d40b7a6963c413312b916a72
Deleted: sha256:0269bb8ab7750b1ccbbcee578df3af6edcba46c367891e319e3b874366c91b2e
Deleted: sha256:5ff949a6a98fa0b9294d5feab73bc2bd31947ecdf546d426d49401a5a5d8b9f4
Deleted: sha256:5e4268f1622d7392ab023aa0c458ee836bda8833e0b0f9d6b3333fff071bb761
```

### Reto Adicional
#### Cambiar de puerto
Para ejecutar la imagen en otro puerto, basta con cambiar el puerto del anfitrión en el comando 
> [!TIP]
> docker run -d -p 8888:8080 --name tomcat-server tomcat

#### Crear un volumen
> [!IMPORTANT]
> Un volumen de docker se utiliza para poder conservar la información de un contenedor, aqunque este sea eliminado.
> Esto nos permite transferir la información de un contenedor a otro e intercambiar datos entre varios contenedores

> [!TIP]
> sudo docker volume create --name tomcat-volume

Creamos un volumen con el nombre <i>**tomcat-volume**</i>
```bash
tomcat-volume
```
---

> [!TIP]
> docker run -d -p 9000:8080 -v tomcat-volume:/data --name tomcat-server tomcat
```bash
37dda271e541a0ecb55bd8db91b676be396a72d6d21f9181184f7a2d05e5e5a0
```
---
> [!TIP]
> sudo docker volume ls

Lista los volumenes registrados.
```bash
DRIVER    VOLUME NAME
local     tomcat-volume
```
---
> [!TIP]
> sudo docker volume inpect tomcat-volume

Proporciona información acerca del volumen
```bash
[
    {
        "CreatedAt": "2024-11-26T22:43:10Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/snap/docker/common/var-lib-docker/volumes/tomcat-volume/_data",
        "Name": "tomcat-volume",
        "Options": null,
        "Scope": "local"
    }
]
```
---
> [!TIP]
> sudo docker stop tomcat-server
>
> sudo docker rm tomcat-server

Primero tenemos que detener y eliminar el contenedor que está utilizando el volumen

---
> [!TIP]
> sudo docker volume rm tomcat-volume

Por último, eliminamos el volumen

## Tarea 3
> [!TIP]
> docker cp ./sample.war tomcat-server:/usr/local/tomcat/webapps/

Copiamos el directorio .war al contenedor de tomcat.
```bash
Successfully copied 7.17kB to tomcat-server:/usr/local/tomcat/webapps/
```
---
> [!CAUTION]
> Esta ruta me da 404 (revisar)
> 
> http://localhost:9000/sample
```bash
```
---
> [!TIP]
> docker logs -f tomcat-server

Ver historial de peticiones de la conexión
```bash
26-Nov-2024 23:21:38.279 INFO [Catalina-utility-2] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/tomcat/webapps/.git] has finished in [15] ms
```
---
> [!TIP]
> docker inspect tomcat-server

Obtener información del contenedor 
> IP: 172.17.0.2 

> Puerto: 8080