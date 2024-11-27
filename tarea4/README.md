## Tarea 4
> [!TIP]
> docker -v
```bash
Docker version 27.2.0, build 3ab4256
```
---
> [!TIP]
> docker ps
```bash
CONTAINER ID   IMAGE     COMMAND             CREATED         STATUS         PORTS                                         NAMES
2ebf82ff44bb   tomcat    "catalina.sh run"   9 seconds ago   Up 8 seconds   0.0.0.0:9000->8080/tcp, [::]:9000->8080/tcp   tomcat-server
```
---
> [!TIP]
> docker run --name mariadb-container -e MYSQL_ROOT_PASSWORD=admin -e MYSQL_DATABASE=exampledb -p 3307:3306 -d mariadb:latest

Creamos un contenedor con la imagen de MariaDB
```bash
WORD=admin -e MYSQL_DATABASE=exampledb -p 3307:3306 -d mariadb:latest
Unable to find image 'mariadb:latest' locally
latest: Pulling from library/mariadb
afad30e59d72: Already exists 
b798007f0f2e: Pull complete 
6848f7678a48: Pull complete 
09caa4361361: Pull complete 
edd0b513c29e: Pull complete 
823030c1f43b: Pull complete 
188db50456bf: Pull complete 
69a30bc484d7: Pull complete 
Digest: sha256:0a620383fe05d20b3cc7510ebccc6749f83f1b0f97f3030d10dd2fa199371f07
Status: Downloaded newer image for mariadb:latest
c26a7e00f783f77ea926be0b20858f1bbb347b35381116a96499943f4abb86d2
```
---
> [!TIP]
> docker ps
```bash
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS                                         NAMES
c26a7e00f783   mariadb:latest   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   0.0.0.0:3307->3306/tcp, [::]:3307->3306/tcp   mariadb-container
```
---
> [!TIP]
> docker ps -a
```bash
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS                    PORTS                                         NAMES
c26a7e00f783   mariadb:latest   "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes              0.0.0.0:3307->3306/tcp, [::]:3307->3306/tcp   mariadb-container
```
---
> [!TIP]
> docker logs -f mariadb-container
```bash
2024-11-27 15:00:29 0 [Note] mariadbd: ready for connections.
Version: '11.6.2-MariaDB-ubu2404'  socket: '/run/mysqld/mysqld.sock'  port: 3306  mariadb.org binary distribution
```
---
> [!TIP]
> docker inspect mariadb-container

Accedemos al contenedor por medio de la ip proporcionada (172.17.0.2)
```bash
La conexión ha sido reiniciada
La conexión al servidor fue reiniciada mientras la página se cargaba.
```

> [!TIP]
> docker run -d --name cloudbeaver -p 8978:8978 dbeaver/cloudbeaver:latest

Creamos un contenedor con la imagen del gestor de bbdd <i>**CloudBeaver**</i>
```bash
Unable to find image 'dbeaver/cloudbeaver:latest' locally
latest: Pulling from dbeaver/cloudbeaver
9c704ecd0c69: Pull complete 
1ecaf9f47d12: Pull complete 
f771417f3e38: Pull complete 
e74eee18d850: Pull complete 
4f4fb700ef54: Pull complete 
364eab3afb5a: Pull complete 
e724359f8e7e: Pull complete 
28cbd2fcb78e: Pull complete 
Digest: sha256:be0b0b8e16a5ba5abbeec3dc18e2bb18e1d2f6bb04135e32faace799a78b17c4
Status: Downloaded newer image for dbeaver/cloudbeaver:latest
ee57db95ac6d5ee71cbb85d55f4bf25b21c3f67ce1659fa9835adbfaf093b06b
```
---
> [!TIP]
> docker ps -a
```bash
CONTAINER ID   IMAGE                        COMMAND                  CREATED              STATUS                    PORTS                                         NAMES
ee57db95ac6d   dbeaver/cloudbeaver:latest   "./run-server.sh"        About a minute ago   Up About a minute         0.0.0.0:8978->8978/tcp, :::8978->8978/tcp     cloudbeaver
```


Por último, accedemos a localhost:8978 (que es dónde se encuentra nuestro gestor de bbdd):
-  Realizamos la configuración inicial
-  Nos logeamos
-  Vamos al **+ -> Find Database** y ponemos la ip del contenedor de mariadb que se encuentra en **docker inspect mariadb-container** (172.17.0.2)
-  Seleccionamos el driver de **MariaDB** y ponemos los datos que declaramos en el **docker run** (ddbb: exampledb, p: 3306, user: root, password: admin) y le damos a test
> [!CAUTION]
> En el paso anterior me da el siguiente error 
```
Error connecting to database:
Connection failed:
Socket fail to connect to host:address=(host=localhost)(port=3306)(type=primary). Connection refused
```
- En caso de que no de error (no es mi caso) creamos la conexión con el la base de datos del contenedor de mariadb

> [!TIP]
> docker stop mariadb-container
>
> docker stop cloudbeaver

Paramos ambos contenedores antes de eliminarlos

> [!TIP]
> docker rm mariadb-container
>
> docker rm cloudbeaver

Eliminamos los contenedores