# Instalación de Netdata con APT

## Introducción
Netdata es una herramienta de monitoreo en tiempo real para sistemas Linux. Permite visualizar métricas del sistema con una interfaz web intuitiva.

Este documento describe cómo instalar Netdata en distribuciones basadas en Debian y Ubuntu utilizando `apt`.

## Requisitos previos
Asegúrate de que tu sistema esté actualizado antes de la instalación:

```bash
sudo apt update && sudo apt upgrade -y
```

## Instalación de Netdata

1. **Añadir el repositorio de Netdata:**

   ```bash
   sudo apt install -y netdata
   ```

2. **Habilitar y arrancar el servicio:**

   ```bash
   sudo systemctl enable netdata
   sudo systemctl start netdata
   ```

3. **Verificar el estado del servicio:**

   ```bash
   systemctl status netdata
   ```

## Acceder a la interfaz web
Por defecto, Netdata se ejecuta en el puerto `19999`. Puedes acceder a la interfaz web desde tu navegador ingresando:

```
http://localhost:19999/
```

Si deseas acceder desde otro dispositivo, usa la dirección IP del servidor en lugar de `localhost`.

## Configuración adicional
Para configurar Netdata, puedes editar su archivo de configuración:

```bash
sudo nano /etc/netdata/netdata.conf
```

Después de realizar cambios, reinicia Netdata:

```bash
sudo systemctl restart netdata
```

## Desinstalar Netdata
Si necesitas eliminar Netdata, ejecuta:

```bash
sudo apt remove --purge netdata -y
```

## Conclusión
Ahora tienes Netdata instalado y funcionando en tu sistema. Puedes usarlo para monitorear métricas del sistema en tiempo real y optimizar el rendimiento de tu servidor.
