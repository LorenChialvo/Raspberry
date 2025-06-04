# Configuración de un Servidor FTP en Raspberry Pi

## Índice
1. [Introducción](#introducción)
2. [Requisitos](#requisitos)
3. [Funcionalidad](#Funcionalidad)
4. [Instalación del servidor FTP (vsftpd)](#instalación-del-servidor-ftp-vsftpd)
5. [Configuración del Servidor FTP en la Raspberry Pi](#Configuración-del-Servidor-FTP-en-la-Raspberry-Pi)
6. [Pruebas de acceso con clientes FTP](#pruebas-de-acceso-con-clientes-ftp)
7. [Pruebas de funcionamiento](#pruebas-de-funcionamiento)
5. [Referencias](#referencias)
6. [Conclusion](#conclusion)
7. [Volver al README](https://github.com/SyT-2024/tp-raspberry-grupo-5/blob/main/README.md)

## Introducción

FTP significa “File Transfer Protocol”, o Protocolo de Transferencia de Archivos. Este protocolo permite el intercambio de datos entre servidores o equipos conectados en una red, ya sea Internet o redes locales (LAN). Su función principal es facilitar la transferencia de archivos, permitiendo que los usuarios envíen y reciban datos de manera rápida y sencilla desde diferentes ubicaciones.

El funcionamiento de FTP se basa en un modelo cliente-servidor. Esto significa que un equipo (el cliente) se conecta a otro equipo (el servidor) para acceder a archivos, cargarlos, descargarlos o gestionarlos. Esta estructura ha hecho de FTP una herramienta versátil y ampliamente adoptada en entornos como la administración de sitios web, el almacenamiento de archivos compartidos y el respaldo de información.

Un servidor FTP, como el que configuramos en esta guía, es esencial en una red local (LAN) cuando varios dispositivos necesitan intercambiar datos sin complicaciones. En el caso de una Raspberry Pi, al instalar y configurar un servidor FTP, esta pequeña computadora puede convertirse en un nodo central para la gestión de archivos en la red, brindando acceso a documentos y datos sin requerir infraestructura adicional. Además, el servidor FTP es compatible con clientes de distintos sistemas operativos, lo que facilita su uso en equipos Windows, macOS y Linux.

Para probar la conexión y la funcionalidad del servidor FTP en la Raspberry Pi, utilizaremos aplicaciones cliente FTP como **FileZilla** y **WinSCP**, que permiten conectar, subir y descargar archivos con facilidad. Estas herramientas son esenciales para verificar que el servidor esté correctamente configurado y para comprobar que los usuarios de la red pueden acceder a los archivos según los permisos establecidos.

## Funcionalidad
¿Para que nos sirve el FTP? Con el FTP podemos subir archivos a un servidor a traves de nuestro computador, bajar archivos de este, administrar el servidor (como por ejemplo, cambiar los permisos de usuario, mover o renombrar archivos al igual que eliminarlos si lo amerita la situación), sincronizar los archivos de un servidor con los archivos de nuestro equipo y automatizar la subida y bajada de archivos del servidor. FTP y sus derivados fueron especialmente diseñados y son ampliamente utilizados por su facilidad para el manejo de serviores de forma remota

## Requisitos
- Raspberry Pi con sistema operativo basado en Linux.
- Conexión de red (LAN o Internet).
- Cliente FTP para pruebas (recomendado: FileZilla).

## Instalación del servidor FTP (vsftpd)
Antes de comenzar,¿Qué es vsftpd? Vsftpd significa Very Secure FTP Daemon, un programa diseñado con la base de FTP que se especializa en la transferencia de archivos de una forma más segura y protegida que FTP. Otras caracteristicas importantes de VSFTPD son su velocidad de subida, compatibilidad con chroot (permitiendo que los usuarios puedan aislarse en sus directorios privados para mayor seguridad), y su ya mencionada seguridad. ¿Como instalamos vsftpd? Para esto seguimos las siguientes pasos:
1. Actualiza los repositorios e instala el servidor `vsftpd`:
   ```bash
   sudo apt update
   sudo apt install vsftpd
   ```
   sudo: se usa sudo para ejecutar el comando con privilegios de superusuario, lo que es necesario para instalar software en    el       
   sistema.
   
   apt: Es el gestor de paquetes para distribuciones de Linux basadas en Debian, como Ubuntu y Raspberry Pi OS. Facilita la instalación,    actualización y eliminación de programas.
   
   update: Es una subcomando de apt que actualiza la lista de paquetes. Al ejecutar apt update, el sistema se conecta a los repositorios    configurados y descarga la información más reciente sobre los paquetes y sus versiones disponibles. No instala ni actualiza ningún       software; solo asegura que la información sobre los paquetes esté actualizada.
   
-----

   sudo: Nuevamente, se usa sudo para ejecutar el comando con privilegios de superusuario, lo que es necesario para instalar software en    el sistema.
   
   apt: El gestor de paquetes, que facilita la instalación de software en el sistema.
   
   install: Es una subcomando de apt que indica que queremos instalar un paquete en el sistema.
   
   vsftpd: Especifica el paquete que queremos instalar, en este caso, vsftpd, que es un servidor FTP. apt buscará este paquete en los       repositorios configurados y descargará e instalará las versiones correspondientes junto con sus dependencias.

3. Inicia el servicio de vsftpd y asegúrate de que se ejecute al iniciar el sistema:
    ```bash
    sudo systemctl start vsftpd
    sudo systemctl enable vsftpd
    ```
   sudo: Permite ejecutar el comando con permisos administrativos, ya que se están realizando cambios en los servicios del sistema.
   
   systemctl: Es una herramienta de administración para controlar servicios y demonios en sistemas Linux que usan systemd como gestor de    servicios. systemctl permite iniciar, detener, reiniciar y verificar el estado de los servicios.
   
   start: Es una subcomando de systemctl que indica que queremos iniciar un servicio específico.
   
   vsftpd: Es el nombre del servicio que queremos iniciar, en este caso, el servidor FTP vsftpd. Este comando pone en funcionamiento el     servidor, haciéndolo accesible en la red.

    --------
   
   sudo: Otorga privilegios administrativos para ejecutar el comando.
   
   systemctl: Herramienta para administrar servicios en el sistema.
   
   enable: Es una subcomando de systemctl que configura un servicio para que se inicie automáticamente al arrancar el sistema. Al           habilitar vsftpd, el servidor FTP se iniciará cada vez que se encienda o reinicie la Raspberry Pi.
   
   vsftpd: Especifica el servicio vsftpd, que queremos habilitar para el inicio automático.
   
## *Configuración del Servidor FTP en la Raspberry Pi*
### *1. Editar el Archivo de Configuración del Servidor FTP*
Para configurar vsftpd de acuerdo a las necesidades de la red, abre el archivo de configuración:
```bash
sudo nano /etc/vsftpd.conf
```

sudo: Ejecuta el comando con privilegios de superusuario (administrador). Esto es necesario para realizar cambios en el sistema, como editar archivos de configuración del sistema.

nano: Es un editor de texto simple y fácil de usar en la línea de comandos. Se utiliza para abrir y editar archivos de texto.

/etc/vsftpd.conf: Es la ruta al archivo de configuración de vsftpd. En este archivo se definen las opciones y parámetros del servidor FTP.

En este archivo tienes que sacar el # en estas lineas:

*listen=YES*  
*listen_ipv6=NO*  
*anonymous_enable=NO*  
*local_enable=YES*  
*write_enable=YES*  
*local_umask=022*  

![imager](/imagenes/ConfigserverFTP1.jpeg)
![imager](/imagenes/ConfigserverFTP2.jpeg)
![imager](/imagenes/ConfigserverFTP3.jpeg)
![imager](/imagenes/ConfigserverFTP4.jpeg)
![imager](/imagenes/ConfigserverFTP5.jpeg)

Cuando hayamos hecho estos cambios podemos salir con <kbd>Ctrl + X</kbd>, nos va a preguntar si queremos guardar los cambios y presionamos <kbd>Y</kbd> y luego <kbd>Enter</kbd> para confirmar el nombre del archivo.

### *2. Reiniciar el Servicio FTP*
Después de hacer cambios en el archivo de configuración, reinicia el servicio de vsftpd para aplicar los ajustes:
```bash
sudo systemctl restart vsftpd
```

sudo: Ejecuta el comando con privilegios de superusuario (administrador). Esto es necesario para gestionar archivos de configuración del sistema que requieren permisos elevados.

systemctl: Es una herramienta de administración en sistemas Linux que se utiliza para iniciar, detener, reiniciar y gestionar servicios y unidades del sistema.

restart: Es una subcomando de systemctl que detiene y vuelve a iniciar el servicio. Esto es útil cuando se han realizado cambios en la configuración del servicio, ya que restart aplica cualquier modificación realizada.

vsftpd: Indica el servicio específico que queremos reiniciar, en este caso, el servidor FTP vsftpd.

### *3. Configuracion de SSHD*
Para configurar el ssh, se debe editar el archivo de configuración:
```bash
sudo nano /etc/ssh/sshd_config
```
sudo: Ejecuta el comando con privilegios de superusuario (administrador). Esto es necesario para gestionar archivos de configuración del sistema que requieren permisos elevados.

nano: Es un editor de texto en línea de comandos que permite crear y modificar archivos de texto en un entorno de terminal.

/etc/ssh/sshd_config: Es la ruta absoluta al archivo de configuración del servidor SSH. Este archivo contiene configuraciones que determinan cómo se comporta el servicio SSH en el sistema.

En este archivo tienes que sacar el # en estas lineas:

*Port 22* 

*PermitRootLogin yes*

*PasswordAuthentication yes*

### *4. Reiniciar SSH*
```bash
sudo systemctl restart ssh
```
sudo: Ejecuta el comando con privilegios de superusuario (administrador). Esto es necesario para gestionar servicios del sistema que requieren permisos elevados.

systemctl: Es una herramienta de administración en sistemas Linux que se utiliza para iniciar, detener, reiniciar y gestionar servicios y unidades del sistema.

restart: Es un subcomando de systemctl que detiene y luego vuelve a iniciar un servicio, aplicando cualquier cambio reciente en su configuración.

ssh: Es el nombre del servicio de SSH (Secure Shell) que se desea reiniciar. Este servicio permite conexiones seguras a través de la red.



## Pruebas de acceso con clientes FTP
Para probar la accesibilidad al servidor FTP de la Raspberry Pi, utiliza un cliente FTP como FileZilla:
### FileZilla
    1. Abre FileZilla y dirígete a Archivo > Gestor de sitios.
    2. Añade una nueva entrada con los siguientes detalles:
        - Host: Dirección IP de la Raspberry Pi.
        - Protocolo: FTP - Protocolo de transferencia de archivos.
        - Tipo de acceso: Normal.
        - Usuario y contraseña: Credenciales de la Raspberry Pi.
    3. Haz clic en Conectar para acceder al servidor.
![imager](/imagenes/PruebasDeConexion.jpeg)
## Pruebas de funcionamiento
![imager](/imagenes/PruebasDeFuncionamiento.jpeg)
## Referencias
Tutorial de Geeky Theory: Servidor FTP en Raspberry Pi (link de la pagina)
Documentación de FileZilla (link de la pagina)
## Conclusion

En este trabajo, configuramos un servidor FTP en la Raspberry Pi utilizando `vsftpd`, que es una solución eficiente y accesible para transferir archivos en redes LAN y, con ajustes adicionales de seguridad, también en redes más amplias. Este tipo de servidor permite a los usuarios subir y descargar archivos desde un punto centralizado, facilitando la colaboración y el acceso a documentos compartidos.

Al utilizar clientes FTP como FileZilla y WinSCP, se puede verificar fácilmente el funcionamiento y la accesibilidad del servidor, logrando un entorno de transferencia de archivos seguro y personalizado. En resumen, la Raspberry Pi, junto con `vsftpd`, proporciona una plataforma versátil y económica para cubrir las necesidades de almacenamiento y acceso a archivos en una red local.
