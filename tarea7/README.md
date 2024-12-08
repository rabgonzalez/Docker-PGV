# Tarea 7

## Indice
- [Tarea 7](#tarea-7)
  - [Indice](#indice)
    - [Crea la red personalizada](#crea-la-red-personalizada)
    - [Crea el volumen en comun](#crea-el-volumen-en-comun)
    - [Crear el Dockerfile](#crear-el-dockerfile)
    - [Paso 4: Construir y ejecutar la imagen](#paso-4-construir-y-ejecutar-la-imagen)
    - [Detener y eliminar contenedores](#detener-y-eliminar-contenedores)

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

### Crear el Dockerfile
> [!TIP]
> docker build -t tomcat-mariadb-cloudbeaver .

```bash
[+] Building 299.5s (5/5) FINISHED      docker:default
 => [internal] load build definition from Docker  0.1s
 => => transferring dockerfile: 1.04kB            0.0s
 => [internal] load metadata for docker.io/dbeav  1.9s
 => [internal] load .dockerignore                 0.1s
 => => transferring context: 2B                   0.0s
 => [stage-3 1/1] FROM docker.io/dbeaver/cloud  296.8s
 => => resolve docker.io/dbeaver/cloudbeaver:lat  0.1s
 => => sha256:e315cf25aa2890df59 4.02MB / 4.02MB  3.5s
 => => sha256:00c4de099c738d72ab 2.58kB / 2.58kB  0.0s
 => => sha256:e3d0d7648095610 19.96MB / 19.96MB  11.0s
 => => sha256:7a5a786cddef6ac06b 1.61kB / 1.61kB  0.0s
 => => sha256:af8c30eabfb7b3bc8c 6.06kB / 6.06kB  0.0s
 => => sha256:5be91d7c9932 144.54MB / 144.54MB  113.6s
 => => sha256:4f4fb700ef54461cfa02571a 32B / 32B  4.7s
 => => sha256:92dd7dd35fcff43 30.58MB / 30.58MB  59.3s
 => => sha256:ce5b95a3232a0dcad 1.25kB / 1.25kB  12.3s
 => => extracting sha256:e3d0d764809561069d8f81  37.8s
 => => sha256:3ed805a14994 213.18MB / 213.18MB  207.2s
 => => sha256:fabf96dd527b9ff6e 2.57kB / 2.57kB  61.0s
 => => sha256:fe52f78ad45b77e10ca06 499B / 499B  61.5s
 => => extracting sha256:5be91d7c99325d4137c9c  110.6s
 => => extracting sha256:e315cf25aa2890df59ab6c6  2.9s
 => => extracting sha256:4f4fb700ef54461cfa02571  0.0s
 => => extracting sha256:92dd7dd35fcff43b1fc9e50  9.5s
 => => extracting sha256:ce5b95a3232a0dcadd25c18  0.0s
 => => extracting sha256:3ed805a149946892299a9a  55.5s
 => => extracting sha256:fabf96dd527b9ff6e765b8f  0.0s
 => => extracting sha256:fe52f78ad45b77e10ca065f  0.0s
 => exporting to image                            0.2s
 => => exporting layers                           0.0s
 => => writing image sha256:38844455e25ebbc82254  0.1s
 => => naming to docker.io/library/tomcat-mariad  0.0s

 2 warnings found (use docker --debug to expand):
 - SecretsUsedInArgOrEnv: Do not use ARG or ENV instructions for sensitive data (ENV "MYSQL_ROOT_PASSWORD") (line 29)
 - JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 33)
```

### Paso 4: Construir y ejecutar la imagen
> [!TIP]
> docker run -d -p 8978:8978 -p 8081:8081 tomcat-mariadb-cloudbeaver

```bash
03099ccae9c0878ce2b18b12541a6a63a424c4ea270148b749bfe30e7f8fe5dd
```

### Detener y eliminar contenedores
> [!TIP]
> docker stop priceless_roentgen
>
> docker rm priceless_roentgen

```bash
priceless_roentgen
```