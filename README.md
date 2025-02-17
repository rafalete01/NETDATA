# Instalación de Netdata con Docker

Este documento proporciona instrucciones para instalar Netdata utilizando Docker. Netdata es una herramienta de monitoreo en tiempo real que ayuda a visualizar el rendimiento de sistemas y aplicaciones.
En nuestro caso hemos utilizado:

Docker Netdata para monitorización de mysql.
Docker Mysql: Base de datos que va a ser monitorizada por Netdata.

## Requisitos Previos

- Tener Docker instalado en tu sistema. Puedes seguir la [guía oficial de instalación de Docker](https://docs.docker.com/get-docker/) 

## Comando de Instalación

Para instalar Netdata, ejecuta el siguiente comando en tu terminal:

```bash
docker run -d --name=netdata \
  --pid=host \
  --network=host \
  -v netdataconfig:/etc/netdata \
  -v netdatalib:/var/lib/netdata \
  -v netdatacache:/var/cache/netdata \
  -v /:/host/root:ro,rslave \
  -v /etc/passwd:/host/etc/passwd:ro \
  -v /etc/group:/host/etc/group:ro \
  -v /etc/localtime:/etc/localtime:ro \
  -v /proc:/host/proc:ro \
  -v /sys:/host/sys:ro \
  -v /etc/os-release:/host/etc/os-release:ro \
  -v /var/log:/host/var/log:ro \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  -v /run/dbus:/run/dbus:ro \
  --restart unless-stopped \
  --cap-add SYS_PTRACE \
  --cap-add SYS_ADMIN \
  --security-opt apparmor=unconfined \
  netdata/netdata

## Ejecutar MySQL en un contenedor Docker

Ejecuta el siguiente comando para iniciar un contenedor de MySQL:

```bash
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=tu_contraseña -d mysql:latest
```

- `--name mysql-container`: le da un nombre al contenedor.
- `-e MYSQL_ROOT_PASSWORD=tu_contraseña`: establece la contraseña de root para MySQL.
- `-d`: ejecuta el contenedor en segundo plano.
- `mysql:latest`: utiliza la última versión de la imagen de MySQL.

## Verificar que el contenedor esté en ejecución

Puedes verificar que los contenedores están corriendo con el siguiente comando:

```bash
docker ps
```

## Conectarse a MySQL

Para conectarte al servidor MySQL que se está ejecutando en el contenedor, usa el siguiente comando:

```bash
docker exec -it mysql-container mysql -u root -p
```

Te pedirá la contraseña que configuraste anteriormente.

## Acceder a MySQL desde tu máquina local

Si deseas acceder a MySQL desde tu máquina local, puedes mapear el puerto del contenedor al puerto de tu máquina. Usa el siguiente comando:

```bash
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:latest
```

# Habilitar el monitoreo de una base de datos MySQL (o MariaDB) desde Netdata.

root@rafabello:/# echo "local: 
> user: 'netdata'
> pass: 'netdata'
> host: 'mariadb'
> port: 3306" > /etc/netdata/python.d/mysql.conf
root@rafabello:/# cat /etc/netdata/python.d/mysql.conf 
local:
user: 'netdata'
pass: 'netdata'
host: 'mariadb'
port: 3306"

# Crear el usuario en MYSQL/ MARIADB
```bash
CREATE USER 'netdata'@'%' IDENTIFIED BY 'netdata';
GRANT USAGE, REPLICATION CLIENT, PROCESS, SELECT ON . TO 'netdata'@'%';
FLUSH PRIVILEGES; 
```
# Verificamos la conectividad
![Descripción de la imagen](/images/netdata.jpg)
