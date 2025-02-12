# Cómo instalar Netdata en Ubuntu 22.04

Netdata es una herramienta de monitoreo en tiempo real de código abierto para servidores. Recopila datos en tiempo real como el uso de la CPU, RAM, carga, uso de SWAP, ancho de banda, uso del disco, etc.

## Actualizar el servidor
Actualicemos el servidor usando el siguiente comando:

```bash
sudo apt update -y
sudo apt upgrade -y
```

## Descargar el paquete de Netdata
Instala Netdata en el servidor utilizando el siguiente comando:

```bash
sudo apt install netdata -y
```

La opción `-y` se usa para confirmar automáticamente la instalación sin necesidad de intervención manual.

## Configuración de Netdata
Es necesario realizar un pequeño cambio en el archivo de configuración para que el panel de control esté disponible en una IP pública.

Si solo lo usarás localmente, no es necesario realizar este cambio.

Abre el archivo de configuración:

```bash
sudo nano /etc/netdata/netdata.conf
```

El archivo de configuración se verá así:

```ini
[global]
    run as user = netdata
    web files owner = root
    web files group = root
    # Netdata no está diseñado para ser expuesto a redes potencialmente hostiles
    # Ver https://github.com/netdata/netdata/issues/164
    bind socket to IP = 127.0.0.1
```

Por defecto, `bind socket to IP` está configurado en `127.0.0.1`. Para acceder al panel de control usando la dirección IP del servidor, reemplaza `127.0.0.1` con la dirección IP de tu servidor.

```ini
[global]
    run as user = netdata
    web files owner = root
    web files group = root
    # Netdata no está diseñado para ser expuesto a redes potencialmente hostiles
    # Ver https://github.com/netdata/netdata/issues/164
    bind socket to IP = <Introduce tu dirección IP aquí>
```

Guarda el archivo y reinicia el servicio de Netdata con el siguiente comando:

```bash
sudo systemctl restart netdata
```

## Configuración del firewall
Netdata escucha en el puerto `19999` por defecto. Habilita este puerto en el firewall para poder acceder al panel desde el navegador:

```bash
sudo ufw allow 19999
```

## Panel de control de Netdata
Ingresa la siguiente URL en tu navegador para acceder al panel de control de Netdata. Por defecto, Netdata funciona en el puerto `19999`.

```
http://<Introduce tu dirección IP aquí>:19999/
```

El panel de control se verá como se muestra a continuación.
