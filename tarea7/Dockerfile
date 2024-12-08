# Usar una imagen base de Ubuntu para las instalaciones adicionales
FROM ubuntu:20.04

# Instalar dependencias necesarias (como wget y curl)
RUN apt-get update -y && \
    apt-get install -y \
    wget \
    curl \
    unzip \
    mysql-client \
    && rm -rf /var/lib/apt/lists/*

# Configurar MariaDB usando la imagen oficial
FROM mariadb:10.5

# Configurar Tomcat usando la imagen oficial
FROM tomcat:9.0

# Descargar y configurar CloudBeaver utilizando la imagen oficial de CloudBeaver desde Docker Hub
FROM dbeaver/cloudbeaver:latest

# Exponer puertos
EXPOSE 8080 8081

# Volúmenes para MariaDB
VOLUME /var/lib/mysql

# Configuración de MariaDB: Establecer la contraseña root y crear la base de datos (esto es suficiente con las variables de entorno)
ENV MYSQL_ROOT_PASSWORD=root
ENV MYSQL_DATABASE=exampledb

# Iniciar los servicios de MariaDB, Tomcat y CloudBeaver
CMD service mysql start && \
    /opt/tomcat/bin/catalina.sh run & \
    /opt/cloudbeaver/cloudbeaver/bin/cloudbeaver & \
    wait