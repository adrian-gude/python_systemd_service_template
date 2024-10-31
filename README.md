# Configuración de un Script de Python como Servicio de `systemd`

Esta guía te ayudará a convertir tu script de Python en un servicio de `systemd`, permitiendo que se ejecute automáticamente en segundo plano y se reinicie en caso de fallos.

## Pasos para convertir tu script en un servicio de `systemd`:

### Verifica que tu script funcione correctamente desde la terminal

Antes de crear el servicio, asegúrate de que tu script se ejecute sin problemas:

```bash
python /ruta/a/tu_script.py
```

## Ubica tu script y el archivo .env en una ruta fija

Coloca tu script y el archivo .env en un directorio específico, por ejemplo:

    Script: /home/tu_usuario/<workspace>/<archivo.py>
    Archivo .env: /home/tu_usuario/<workspace>/<.env>

## Crear el archivo de servicio de systemd

Crea un archivo llamado <myservice>.service en el directorio **/etc/systemd/system/**
```bash
sudo nano /etc/systemd/system/<my_service>.service
```

## Contenido del servicio
```bash
[Unit]
Description=Descripción del servicio
After=network.target

[Service]
Type=simple
User=tu_usuario
WorkingDirectory=<path de la carpeta del proyecto>
ExecStart=<path/entorno/virtual> </path/script.py>
Restart=on-failure
EnvironmentFile=<path/variables/entorno>

[Install]
WantedBy=multi-user.target
```

Explicación de los campos:
- Description: Una breve descripción del servicio.
- After: Asegura que el servicio se inicie después de que la red esté disponible.
- User: El usuario bajo el cual se ejecutará el servicio. Reemplaza tu_usuario con tu nombre de usuario.
    WorkingDirectory: El directorio donde se encuentra tu script y el archivo .env.
- ExecStart: El comando para iniciar tu script. Asegúrate de que la ruta a Python 3 sea correcta.
- Restart: Configura el servicio para que se reinicie en caso de fallo.
- EnvironmentFile: Especifica el archivo .env que contiene las variables de entorno.
- WantedBy: Indica el nivel de destino para iniciar este servicio.


## Recargar los archivos de configuración de systemd

Después de guardar el archivo, ejecuta:
```bash
sudo systemctl daemon-reload
```

## Iniciar y habilitar el servicio
### Para iniciar el servicio:
```bash
sudo systemctl start <myservice>.service
```
### Para habilitar el servicio al inicio del sistema:
```bash
sudo systemctl enable <myservice>.service
```


## Verificar el estado del servicio
Puedes comprobar si el servicio está activo:
```bash 
sudo systemctl status <my_service>.service
```

## Ver los logs del servicio
Para ver los registros generados por tu script:
```bash 
sudo journalctl -u <my_service>.service -f
```
