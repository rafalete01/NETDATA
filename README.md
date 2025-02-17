# Instalación de Netdata con Docker

Este documento proporciona instrucciones para instalar Netdata utilizando Docker. Netdata es una herramienta de monitoreo en tiempo real que ayuda a visualizar el rendimiento de sistemas y aplicaciones.

## Requisitos Previos

- Tener Docker instalado en tu sistema. Puedes seguir la [guía oficial de instalación de Docker](https://docs.docker.com/get-docker/) si aún no lo has hecho.

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

  
