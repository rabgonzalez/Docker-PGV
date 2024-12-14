# Tarea 8

## Indice
- [Tarea 8](#tarea-8)
  - [Indice](#indice)
    - [Crear el Docker compose](#crear-el-docker-compose)
      - [Crear las networks y los volumenes](#crear-las-networks-y-los-volumenes)
      - [Crear el servicio de mariadb y su cliente](#crear-el-servicio-de-mariadb-y-su-cliente)
      - [Crear el servicio de mongodb y su cliente](#crear-el-servicio-de-mongodb-y-su-cliente)
      - [Crear los tomcats](#crear-los-tomcats)
    - [Crear el fichero nginx.conf uniendo los tomcats](#crear-el-fichero-nginxconf-uniendo-los-tomcats)
      - [Crear el servicio de nginx usando el fichero nginx.conf](#crear-el-servicio-de-nginx-usando-el-fichero-nginxconf)
      - [Levantar el Docker compose](#levantar-el-docker-compose)
    - [Comprobar que se haya levantado](#comprobar-que-se-haya-levantado)
    - [Detener y eliminar el Docker compose](#detener-y-eliminar-el-docker-compose)

### Crear el Docker compose
#### Crear las networks y los volumenes
```yml
volumes:
  mariadb_volume:
  mongodb_volume:
networks:
  tarea8_network:
    driver: bridge
```

#### Crear el servicio de mariadb y su cliente
```yml
# MARIADB
  mariadb:
    image: mariadb:11.1.2
    container_name: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: example-mariadb
    volumes:
      - mariadb_volume:/var/lib/mysql
    ports:
      - "3307:3306"
    networks:
      - tarea8_network

  cloudbeaver:
    image: dbeaver/cloudbeaver:23.3.0
    container_name: cloudbeaver
    ports:
      - "8978:8978"
    networks:
      - tarea8_network
    depends_on:
      - mariadb
```

#### Crear el servicio de mongodb y su cliente
```yml
# MONGO
  mongodb:
    image: mongo:latest
    container_name: mongodb
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: ${MONGO_INITDB_ROOT_PASSWORD}
    volumes:
      - mongodb_volume:/data/db
    ports:
      - "27017:27017"
    networks:
      - tarea8_network
  
  mongo-express:
    image: mongo-express:latest
    container_name: mongo-express
    environment:
      ME_CONFIG_MONGODB_SERVER: mongodb
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: ${ME_CONFIG_MONGODB_ADMINPASSWORD}
      ME_CONFIG_BASICAUTH: "false"
    ports:
      - "8081:8080"
    networks:
      - tarea8_network
    depends_on:
      - mongodb
```

#### Crear los tomcats
```yml
# TOMCAT
  tomcat1:
    image: tomcat:10.1.9-jdk17
    container_name: tomcat1
    volumes:
      - ./sample.war:/usr/local/tomcat/webapps/sample.war
    ports:
      - "8090:8080"
    networks:
      - tarea8_network
      
  tomcat2:
    image: tomcat:10.1.9-jdk17
    container_name: tomcat2
    volumes:
      - ./sample.war:/usr/local/tomcat/webapps/sample.war
    ports:
      - "8091:8080"
    networks:
      - tarea8_network
```

### Crear el fichero nginx.conf uniendo los tomcats
```js
events {}

http {
    upstream tomcat_backend {
        server tomcat1:8080;
        server tomcat2:8080;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://tomcat_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```

#### Crear el servicio de nginx usando el fichero nginx.conf
```yml
# NGINX
  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "81:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    networks:
      - tarea8_network
    depends_on:
      - cloudbeaver
      - mongo-express
      - tomcat1
      - tomcat2
```

#### Levantar el Docker compose
> [!TIP]
> docker-compose up --build

```bash
[+] Running 10/10
 ✔ Network tarea8_tarea8_network   Created                                                                                                                          0.3s 
 ✔ Volume "tarea8_mongodb_volume"  Created                                                                                                                          0.0s 
 ✔ Volume "tarea8_mariadb_volume"  Created                                                                                                                          0.0s 
 ✔ Container tomcat2               Created                                                                                                                          0.3s 
 ✔ Container mongodb               Created                                                                                                                          0.4s 
 ✔ Container mariadb               Created                                                                                                                          0.3s 
 ✔ Container tomcat1               Created                                                                                                                          0.4s 
 ✔ Container cloudbeaver           Created                                                                                                                          0.2s 
 ✔ Container mongo-express         Created                                                                                                                          0.2s 
 ✔ Container nginx                 Created                                                                                                                          0.1s 
Attaching to cloudbeaver, mariadb, mongo-express, mongodb, nginx, tomcat1, tomcat2
```

### Comprobar que se haya levantado
> [!TIP]
> docker ps

```bash
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS         PORTS                                         NAMES
8c30662d0247   nginx:latest                 "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   0.0.0.0:81->80/tcp, [::]:81->80/tcp           nginx
0ef379a9334e   dbeaver/cloudbeaver:23.3.0   "./run-server.sh"        4 minutes ago   Up 4 minutes   0.0.0.0:8978->8978/tcp, :::8978->8978/tcp     cloudbeaver
8ff0390a32a1   tomcat:10.1.9-jdk17          "catalina.sh run"        4 minutes ago   Up 4 minutes   0.0.0.0:8090->8080/tcp, [::]:8090->8080/tcp   tomcat1
680a0d673a7c   mariadb:11.1.2               "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   0.0.0.0:3307->3306/tcp, [::]:3307->3306/tcp   mariadb
63be5ea259c7   tomcat:10.1.9-jdk17          "catalina.sh run"        4 minutes ago   Up 4 minutes   0.0.0.0:8091->8080/tcp, [::]:8091->8080/tcp   tomcat2
```
---
> [!TIP]
> docker images

```bash
REPOSITORY                   TAG            IMAGE ID       CREATED         SIZE
nginx                        latest         66f8bdd3810c   2 weeks ago     192MB
mongo                        latest         df3f01eba940   7 weeks ago     854MB
mongo-express                latest         870141b735e7   9 months ago    182MB
mariadb                      11.1.2         f35870862d64   14 months ago   404MB
tomcat                       10.1.9-jdk17   0fd87ea2d05c   18 months ago   481MB
```
---
> [!TIP]
> docker volume ls
```bash
DRIVER    VOLUME NAME
local     tarea8_mariadb_volume
local     tarea8_mongodb_volume
```
---
> [!TIP]
> docker network ls
```bash
NETWORK ID     NAME                    DRIVER    SCOPE
48ee7e15bd28   bridge                  bridge    local
c701978f7a3f   host                    host      local
ffd948864783   tarea8_tarea8_network   bridge    local
5731ac2c4f26   tomcat-network          bridge    local
```

### Detener y eliminar el Docker compose
> [!TIP]
> docker-compose down -v
```bash
[+] Running 10/10
 ✔ Container nginx                Removed                                                                                                                           0.0s 
 ✔ Container tomcat1              Removed                                                                                                                           0.0s 
 ✔ Container mongo-express        Removed                                                                                                                           0.0s 
 ✔ Container cloudbeaver          Removed                                                                                                                           0.0s 
 ✔ Container tomcat2              Removed                                                                                                                           0.0s 
 ✔ Container mariadb              Removed                                                                                                                           0.0s 
 ✔ Container mongodb              Removed                                                                                                                           0.0s 
 ✔ Volume tarea8_mariadb_volume   Removed                                                                                                                           0.0s 
 ✔ Volume tarea8_mongodb_volume   Removed                                                                                                                           0.0s 
 ✔ Network tarea8_tarea8_network  Removed                                                                                                                           0.8s
```