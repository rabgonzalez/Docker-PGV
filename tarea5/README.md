## Tarea 5
> [!TIP]
> docker network ls

Lista las redes que van a usar tus contenedores Docker.
```bash
NETWORK ID     NAME      DRIVER    SCOPE
da845d0430b4   bridge    bridge    local
c701978f7a3f   host      host      local
b07fc71362dd   none      null      local
```
---
> [!TIP]
> docker network create tomcat-network

Creamos una nueva red de Docker llamada **tomcat-network** para que los contenedores puedan comunicarse.
```bash
5731ac2c4f2672974bffb5f689395710df4be3797a2ec09635f5904805e442ab
```
---
> [!TIP]
> docker run -d --name tomcat1 --network tomcat-network -p 8081:8080 tomcat:latest
>
> docker run -d --name tomcat2 --network tomcat-network -p 8082:8080 tomcat:latest

Creamos 2 contenedores que utilizarán la misma red local **tomcat-network**
```bash
1abf790b5eac0bc5fb870e3dcf74e11a392754de1041b24c674ebc523dc880ee

fdda609754f11a1b5f330b53a26d0ce4c1a5d7be9d062e62deeceb4d93dab0ec
```
---
> [!TIP]
> docker ps -a
```bash
CONTAINER ID   IMAGE           COMMAND             CREATED              STATUS                  PORTS                                         NAMES
fdda609754f1   tomcat:latest   "catalina.sh run"   About a minute ago   Up About a minute       0.0.0.0:8082->8080/tcp, [::]:8082->8080/tcp   tomcat2
1abf790b5eac   tomcat:latest   "catalina.sh run"   5 minutes ago        Up 5 minutes            0.0.0.0:8081->8080/tcp, [::]:8081->8080/tcp   tomcat1
```
---
> [!TIP]
> docker run -d --name nginx --network tomcat-network -p 81:80 -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf nginx:latest

> [!NOTE]
> nginx.conf guarda la dirección de los contenedores en tomcat_backend (busca el contenedor con ese nombre y lo sistituye por su ip, obteniendo su dirección *(172.18.0.2:8080)*) y gestiona a cúal se conecta en función de los recursos o de si hay errores, está apagada, etc...

Una vez creado el nginx.conf, creamos un contenedor llamado nginx en la misma red que guardará el archivo nginx de nuestra máquina en el directorio nginx del contenedor.
```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
2d429b9e73a6: Already exists 
20c8b3871098: Pull complete 
06da587a7970: Pull complete 
f7895e95e2d4: Pull complete 
7b25f3e99685: Pull complete 
dffc1412b7c8: Pull complete 
d550bb6d1800: Pull complete 
Digest: sha256:0c86dddac19f2ce4fd716ac58c0fd87bf69bfd4edabfd6971fb885bafd12a00b
Status: Downloaded newer image for nginx:latest
07f283236b007ba4f134c2e4bec66e2707d39bdc6d3d37727085ce69b81a2ace
```
---
- http://localhost:8081: tomcat1
- http://localhost:8082: tomcat2
- http://localhost: como no se podía poner el tomcat en el puerto 80 ya que estaba ocupado, localhost nos muestra el puerto 80.
- http://localhost:81: muestra alguno de los dos tomcats (8081-8082).
---
> [!TIP]
> docker stop nginx
```bash
nginx
```
---
> [!TIP]
> docker rm nginx
```bash
nginx
```