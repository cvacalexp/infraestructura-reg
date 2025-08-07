# Instrucciones de despliegue con Docker Compose

Este proyecto utiliza Docker Compose para desplegar los siguientes servicios: SQL Server, Redis, MongoDB, Redis Commander, Mongo Express y Soketi.

## Requisitos previos

Asegúrate de tener instalados los siguientes requisitos previos:

- [Instalación de Docker](https://docs.docker.com/get-docker/)
- [Instalación de Docker Compose](https://docs.docker.com/compose/install/)

## Pasos para el despliegue

1. Crea la red docker para que los servicios se comuniquen entre ellos:
   ```bash
   docker network create network_name
   ```
   
2. Clona este repositorio en tu máquina local:

   ```bash
   git clone https://git.dhl.com/EXPAM-CRA10/infraestructura-reg
   ```

3. Accede al directorio del proyecto:
   ```bash
   cd infraestructura-reg/common-services
   ```

4. Configura tu ambiente local:

   - Copia el archivo **docker-compose.override.yml.example** y 
      pégalo con el nombre **docker-compose.override.yml**.
   - Configura por cada servicio las secciones que desees cambiar como variables de entorno, 
      puertos, credenciales, etc. Puedes apoyarte de la 
      [documentación oficial de Docker Compose](https://docs.docker.com/compose/).
   - Ten en cuenta que los servicios `redis_commander` y `mongo_express` dependen de los servicios
      `redis` y `mongodb` respectivamente, por lo cual las credenciales, urls y puertos 
      deben estar acorde a estos.
   - Puedes hacer uso de los profiles para desactivar servicios que no necesites, en el ejemplo
      se desactiva el servicio `soketi` con lo cual este no se ejecuta a menos que
      agregues el argumento `--profile soketi` al comando de ejecución de docker-compose.
   - Configura el nombre de la red creada en el paso 1.


5. Ejecuta el comando docker-compose up -d para iniciar los servicios en segundo plano:
   ```bash
   docker-compose up -d
   ```
   Esto creará y/o ejecutará los contenedores de Docker para cada uno de los servicios especificados.

   Si quieres ejecutar servicios que estén en un profile determinado puedes correr lo siguiente:
   ```bash
   docker-compose --profile soketi up -d
   ```
   Esto creará y/o ejecutará los contenedores de Docker para cada uno de los servicios especificados
   incluyendo el servicio de Soketi.

6. Espera a que los servicios se desplieguen completamente. 
   Puedes verificar el estado de los contenedores:
   ```bash
   docker ps
   ```

7. Una vez que los servicios estén en ejecución, puedes acceder a los contenedores: 
   ```bash
   docker exec -it container_name bash
   ```
   
   o

   ```bash
   docker exec -it container_name bin/sh
   ```
   
   También puedes acceder a sus interfaces mediante las siguientes URL:

   - SQL Server: localhost:1433
   - Redis: localhost:6379
   - Redis Commander: localhost:8081
   - MongoDB: localhost:27017
   - Mongo Express: localhost:7071
   - Soketi: localhost:6001
   
## Detener o eliminar los servicios

Si deseas detener los servicios puedes ejecutar el siguiente comando:
   ```bash
   docker-compose stop
   ```

Esto detendrá los contenedores.

O si deseas eliminar los contenedores, puedes ejecutar el siguiente comando:
   ```bash
   docker-compose down
   ```

Esto eliminará los contenedores, pero conservará los volúmenes de datos definidos en los archivos de configuración.