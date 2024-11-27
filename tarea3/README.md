## Tarea 3
> [!TIP]
> docker cp ./sample.war tomcat-server:/usr/local/tomcat/webapps/

Copiamos el directorio .war al contenedor de tomcat.
```bash
Successfully copied 7.17kB to tomcat-server:/usr/local/tomcat/webapps/
```
---
> [!TIP]
> http://localhost:9000/sample
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
