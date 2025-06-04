
# Instalación del Sistema Operativo en Raspberry Pi y Configuración del Servicio SSH

## Índice
1. [Introducción](#introducción)
2. [Objetivo del Informe](#objetivo-del-informe)
3. [Desarrollo Teorico](#desarrollo-teorico)
4. [Pasos para Instalar el Sistema Operativo](#pasos-para-instalar-el-sistema-operativo)
5. [Conexion al Servicio SSH](#conexion-al-servicio-ssh)
6. [Conclusión](#conclusión)
7. [Volver al README](https://github.com/SyT-2024/tp-raspberry-grupo-5/blob/main/README.md)

## Introducción
En esta primera entrega instalamos el sistema operativo de nuestra Raspberry Pi. activamos **SSH** (Secure Shell), que nos permitirá conectarnos de manera remota.


## Objetivo del Informe
El objetivo de este informe es detallar el proceso que seguimos para instalar el sistema operativo en la Raspberry Pi, sin entorno gráfico, y cómo activamos el servicio **SSH**. Además, explicaremos los conceptos más importantes relacionados con la Raspberry Pi y **SSH**.

## Desarrollo Teorico
La **Raspberry Pi** es una pequeña computadora que puede utilizarse para muchas aplicaciones. En este caso, instalamos una versión ligera de su sistema operativo, sin interfaz gráfica, lo que nos permitirá gestionarla y correr servicios de manera eficiente.

El **SSH** es un protocolo que nos va a permitir conectarnos a la **Raspberry Pi** de manera segura


## Pasos para Instalar el Sistema Operativo

   
1. **Instalacion del sistema**: Usamos el programa **Raspberry Pi Imager** para grabar la imagen del sistema operativo en la tarjeta microSD. Nos aseguramos de seleccionar la versión sin entorno gráfico.
   ```bash
      sudo apt install rpi-imager
    ``` 
    ![imager](/imagenes/menu_imager.jpeg)
    ![imager](/imagenes/elegir_sis.jpeg)
   - `sudo`: Ejecutamos el comando con permisos de superusuario.
   - `apt`: Utilizamos este gestor de paquetes.
   - `install`: Este parámetro nos permitió instalar el programa.
   - `rpi-imager`: El programa que usamos para instalar el sistema operativo
2. **Configuración inicial**: Una vez que insertamos la tarjeta microSD en la Raspberry Pi, conectamos el dispositivo a la corriente y a la red.

3. **Activar SSH**: Lo primero que hacemos al iniciar la **Raspberry Pi** es actualizar los paquetes
      ```bash
      sudo apt update
      sudo apt upgrade
    ``` 
   - `sudo`: Ejecutamos el comando con permisos de superusuario.
   - `apt`: Utilizamos este gestor de paquetes.
   - `update`: Actualiza la lista de paquetes disponibles en los repositorios (no instala nada).
   - `Upgrade`: Instala las actualizaciones de los paquetes ya instalados, siempre que no haya cambios drásticos en las dependencias
- Luego tenemos que hacer el comando:
   ```bash
      hostname -I
      sudo raspi-config
    ``` 
   ![Obtener ip](/imagenes/IP.jpeg)
   ![Menu raspberry](/imagenes/MENU.jpeg)
   - `hostname`: Este es un comando utilizado para mostrar o configurar el nombre del host del sistema. Un "hostname" es un nombre que identifica a una máquina en una red.

   - `-I`: Esta opción específica del comando hostname indica que se debe mostrar la dirección IP asociada al sistema:
   - `sudo`: Ejecutamos el comando con permisos de superusuario.
   - `raspi-config`: Es la herramienta que abre el menú de configuración interactivo.

    en la terminal del **Raspberry Pi** para conocer nuestra direccion IP y acceder a un menu en el cual vamos a poder activar el protocolo **SSH** 
   ![Menu ssh](/imagenes/SSH.jpeg)
## Conexion al Servicio SSH

1. **Acceder a la Raspberry Pi por SSH**: Después de haber iniciado el servicio **SSH** tenemos que ir a nuestra computadora principal y usar el comando:
   ```bash
      ssh grupo5@ 192.168.60.40
    ``` 

   - `ssh`: Comando que inicia una conexión segura con el servidor utilizando el protocolo SSH.
   - `grupo5`: Es el nombre de usuario en el dispositivo remoto.
   - `192.168.60.40`: Es la dirección IP del dispositivo remoto al cual nos estamos conectando.

   Con ese comando ya nos habriamos conectado exitosamente a la **Raspberry Pi**
   ![Menu ssh](/imagenes/camandossh.png)
## Conclusión
Durante esta entrega, logramos instalar el sistema operativo de nuestra Raspberry Pi y configuramos el servicio **SSH**. Esto nos permitirá conectarnos de manera remota y gestionar el dispositivo para las próximas tareas del proyecto.
