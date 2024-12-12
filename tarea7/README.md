# Tarea 7

## Indice
- [Tarea 7](#tarea-7)
  - [Indice](#indice)
    - [Crea la red personalizada](#crea-la-red-personalizada)
    - [Crea el volumen en comun](#crea-el-volumen-en-comun)
    - [Crear un docker-compose para levatnar todos los servicios](#crear-un-docker-compose-para-levatnar-todos-los-servicios)
    - [Conectarnos al tomcat](#conectarnos-al-tomcat)
    - [Detener y eliminar servicios](#detener-y-eliminar-servicios)

### Crea la red personalizada
> [!TIP]
> docker network create tarea7-network

```bash
3a897940e95175c26e9490d84714d4f6b7c6a44b4d876585138ea7595219fbfc
```

### Crea el volumen en comun
> [!TIP]
> docker volume create tarea7-volume

```bash
tarea7-volume
```

### Crear un docker-compose para levatnar todos los servicios
```yml
version: '3.9'
services:
  mariadb:
    image: mariadb:11.1.2
    container_name: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: exampledb
    volumes:
      - mariadb_data:/var/lib/mysql
    ports:
      - "3307:3306"
    networks:
      - mariadb_network

  tomcat:
    image: tomcat:10.1.9-jdk17
    container_name: tomcat
    ports:
      - "8080:8080"
    volumes:
      - ./sample.war:/usr/local/tomcat/webapps/sample.war
    networks:
      - mariadb_network

  cloudbeaver:
    image: dbeaver/cloudbeaver:23.3.0
    container_name: cloudbeaver
    ports:
      - "8978:8978"
    networks:
      - mariadb_network

volumes:
  mariadb_data:
networks:
  mariadb_network:
```
---
> [!TIP]
> docker-compose up --build

```bash
[+] Running 5/5
 ✔ Network tarea7_default        Created          0.3s 
 ✔ Volume "tarea7_mariadb_data"  Created          0.0s 
 ✔ Container mariadb             Created          0.9s 
 ✔ Container tomcat              Created          0.9s 
 ✔ Container cloudbeaver         Created          0.9s
```

### Conectarnos al tomcat
> [!TIP]
> localhost:8090/sample

> [!CAUTION]
> Este comando da error (no debería)

### Detener y eliminar servicios
> [!TIP]
> docker-compose down -v

```bash
[+] Running 5/5
 ✔ Container cloudbeaver       Removed                                                                                                                             10.4s 
 ✔ Container mariadb           Removed                                                                                                                              0.0s 
 ✔ Container tomcat            Removed                                                                                                                              0.4s 
 ✔ Volume tarea7_mariadb_data  Removed                                                                                                                              0.0s 
 ✔ Network tarea7_default      Removed
```