# Tarea 6

## Indice
- [Tarea 6](#tarea-6)
  - [Indice](#indice)
    - [Crear una red personalizada](#crear-una-red-personalizada)
    - [Crear un volumen para MongoDB](#crear-un-volumen-para-mongodb)
    - [Levantar el contenedor MongoDB](#levantar-el-contenedor-mongodb)
    - [Levantar el Contenedor Mongo Express](#levantar-el-contenedor-mongo-express)
    - [Verificar los contenedores activos](#verificar-los-contenedores-activos)
    - [Vemos los logs del contenedor de mongo express](#vemos-los-logs-del-contenedor-de-mongo-express)
    - [Detener y Eliminar el contenedor](#detener-y-eliminar-el-contenedor)
---
> [!TIP]
> docker network ls

Lista las redes que van a usar tus contenedores Docker.
```bash
NETWORK ID     NAME             DRIVER    SCOPE
f919cc783701   bridge           bridge    local
c701978f7a3f   host             host      local
b07fc71362dd   none             null      local
5731ac2c4f26   tomcat-network   bridge    local
```
---
### Crear una red personalizada
> [!TIP]
> docker network create mongodb-network

Creamos una nueva red de Docker llamada **mongodb-network**.
```bash
1dcbe7c3aa65ab0c03e9f374e5a5e3d70a67b459a502cc42093ad1d6c281bf81
```
---
### Crear un volumen para MongoDB
> [!TIP]
> docker volume create mongodb-data

Creamos un volumen llamado mongodb-data
```bash
mongodb-data
```
---
> [!TIP]
> docker volume ls

Listamos los volúmenes creados
```bash
DRIVER    VOLUME NAME
local     mongodb-data
```
---
### Levantar el contenedor MongoDB
> [!TIP]
> docker run -d --name mongodb-container \
> --network mongodb-network \
> -e MONGO_INITDB_ROOT_USERNAME=admin \
> -e MONGO_INITDB_ROOT_PASSWORD=admin123 \
> -v mongodb-data:/data/db \
> -p 27017:27017 \
> mongo:latest

Creamos un contenedor con lo siguiente:
- red: mongodb-network
- variables: 
  - usuario: admin
  - contraseña: admin123
```bash
Unable to find image 'mongo:latest' locally
latest: Pulling from library/mongo
afad30e59d72: Pull complete 
2ab913c649fa: Pull complete 
142bff30356f: Pull complete 
ea6a78a8bfa5: Pull complete 
e87de320d14a: Pull complete 
e8fb995504bd: Pull complete 
edbe36f78898: Pull complete 
1f774f57f04f: Pull complete 
Digest: sha256:c165af1a407eefce644877bf5a59ba3d9ca762e62b4f1723c919dc08dc32f4d0
Status: Downloaded newer image for mongo:latest
48da659745194445374b3acade656f332f182f404a31b668a8a965b415859562
```
---
### Levantar el Contenedor Mongo Express
> [!TIP]
> docker run -d --name mongo-express-container \
> --network mongodb-network \
> -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
> -e ME_CONFIG_MONGODB_ADMINPASSWORD=admin123 \
> -e ME_CONFIG_MONGODB_SERVER=mongodb-container \
> -p 8081:8081 \
> mongo-express:latest

creamos un contenedor de mongo con express
```bash
Unable to find image 'mongo-express:latest' locally
latest: Pulling from library/mongo-express
619be1103602: Pull complete 
7e9a007eb24b: Pull complete 
5189255e31c8: Pull complete 
88f4f8a6bc8d: Pull complete 
d8305ae32c95: Pull complete 
45b24ec126f9: Pull complete 
9f7f59574f7d: Pull complete 
0bf3571b6cd7: Pull complete 
Digest: sha256:1b23d7976f0210dbec74045c209e52fbb26d29b2e873d6c6fa3d3f0ae32c2a64
Status: Downloaded newer image for mongo-express:latest
e505867f04032b8eb2563c77cad8bfe6b39b88bb1272317fae95983a757484bc
```
---
### Verificar los contenedores activos
> [!TIP]
> docker ps

```bash
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                                           NAMES
e505867f0403   mongo-express:latest   "/sbin/tini -- /dock…"   2 minutes ago   Up 2 minutes   0.0.0.0:8081->8081/tcp, :::8081->8081/tcp       mongo-express-container
48da65974519   mongo:latest           "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   mongodb-container
```
---
### Vemos los logs del contenedor de mongo express
> [!TIP]
> docker logs mongo-express-container

```bash
Mongo Express server listening at http://0.0.0.0:8081
```
---
> localhost:8081
> - contraseña: pass
> - usuario: admin

Accedemos al cliente mongo express 
<img src="../img/localhost:8081.png">

Creamos una nueva base de datos llamada testdb
<img src="../img/mongo-express.png">

Le damos a View para editar la tabla
<img src="../img/testdb.png">

Le damos a new Document
<img src="../img/users.png">

Escribimos el json del usuario y le damos a guardar 
<img src="../img/create-user-mongo-express.png">

Comprobamos que se haya añadido
<img src="../img/create-user-success.png">

---
> [!TIP]
> docker network inspect mongodb-network

Muestra información acerca de la red seleccionada
```bash
[
    {
        "Name": "mongodb-network",
        "Id": "1dcbe7c3aa65ab0c03e9f374e5a5e3d70a67b459a502cc42093ad1d6c281bf81",
        "Created": "2024-12-02T19:59:23.301418003Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```
---
> [!TIP]
> docker exec -it mongodb-container mongosh -u admin -p admin123

Ejecutamos el contenedor desde la terminal.
```bash
Current Mongosh Log ID: 674f220a7f7cd621e5c1c18b
Connecting to:          mongodb://<credentials>@127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.3.3
Using MongoDB:          8.0.3
Using Mongosh:          2.3.3

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.

------
   The server generated these startup warnings when booting
   2024-12-03T15:21:33.236+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2024-12-03T15:21:33.727+00:00: Soft rlimits for open file descriptors too low
   2024-12-03T15:21:33.727+00:00: For customers running the current memory allocator, we suggest changing the contents of the following sysfsFile
   2024-12-03T15:21:33.727+00:00: For customers running the current memory allocator, we suggest changing the contents of the following sysfsFile
   2024-12-03T15:21:33.727+00:00: We suggest setting the contents of sysfsFile to 0.
   2024-12-03T15:21:33.727+00:00: Your system has glibc support for rseq built in, which is not yet supported by tcmalloc-google and has critical performance implications. Please set the environment variable GLIBC_TUNABLES=glibc.pthread.rseq=0
------
```
> [!TIP]
> use testdb

Nos conectamos a la base de datos
```bash
switched to db testdb
```
---
> [!TIP]
> show collections

Muestra todas las tablas
```bash
delete_me
users
```
---
> [!TIP]
> db.users.insertOne({
> 
> name: "Pepe",
> 
> email: "quiero-ser-como-pepe@example.com",
> 
> age: 65
> 
> })

Insertamos un usuario en la tabla db.users
```bash
{
  acknowledged: true,
  insertedId: ObjectId('674f26287f7cd621e5c1c18c')
}
```
---
> [!TIP]
> db.users.find()

Muestra todos los usuarios de la tabla 
(Se puede buscar por id introduciendolo en el find())
> **exit** para salir de la terminal
```bash
[
  {
    _id: ObjectId('674e187e967158b37afd24f7'),
    name: 'John Doe',
    email: 'john@example.com',
    age: 30
  },
  {
    _id: ObjectId('674f26287f7cd621e5c1c18c'),
    name: 'Pepe',
    email: 'quiero-ser-como-pepe@example.com',
    age: 65
  }
]
```
### Detener y Eliminar el contenedor
---
> [!TIP]
> docker stop mongodb-container
>
> docker rm mongodb-container
```bash
mongodb-container
```