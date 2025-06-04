# Configuración de un Servidor DHCP en Raspberry Pi

## Índice
1. [Introducción](#introducción)
2. [Objetivo](#objetivo)
3. [Pasos para Configurar el Servidor DHCP](#pasos-para-configurar-el-servidor-dhcp)
    - [ Actualización del Sistema](#actualización-del-sistema)
    - [ Instalación del Paquete DHCP](#instalación-del-paquete-dhcp)
    - [ Configuración de DHCP en la terminal](#configuración-de-dhcp-en-la-terminal)
    - [ Configuración de Interfaz en la terminal](#configuración-de-interfaz-en-la-terminal)
    - [ Configuración de Red Estática en la terminal](#configuración-de-red-estática-en-la-terminal)
    - [ Reinicio y Comprobación del Servidor DHCP](#reinicio-y-comprobación-del-servidor-dhcp)
4. [Conclusión](#conclusión)
5. [Volver al README](https://github.com/SyT-2024/tp-raspberry-grupo-5/blob/main/README.md)

---

## Introducción
En este informe, configuraremos un servidor **DHCP** básico en una **Raspberry Pi** para administrar las direcciones IP de una red local (LAN) en cada mesa de trabajo. La Raspberry Pi asignará automáticamente las direcciones IP a los dispositivos conectados a la red, formando una red interna controlada por el servidor DHCP. Esta red se visualizará mediante un diagrama en **Packet Tracer**.

## Objetivo
El objetivo es configurar la Raspberry Pi como servidor DHCP de una red LAN específica para cada grupo, permitiendo una administración centralizada de las direcciones IP. Así, los dispositivos conectados al mismo switch de la Raspberry recibirán sus direcciones IP automáticamente desde el servidor DHCP.

---

## Pasos para Configurar el Servidor DHCP

### Actualización del Sistema
Antes de comenzar, es recomendable actualizar los paquetes del sistema:
```bash
sudo apt update
sudo apt upgrade
```
- sudo: Ejecuta el comando con privilegios de superusuario.
- apt: Utiliza el gestor de paquetes para obtener actualizaciones.
- update: Actualiza la lista de paquetes disponibles.
- upgrade: Instala las actualizaciones de los paquetes instalados.

### Instalación del Paquete DHCP
Instalamos el paquete isc-dhcp-server que nos permitirá configurar el servidor DHCP en la Raspberry Pi:
```bash
sudo apt install isc-dhcp-server
```
- sudo: Permite la ejecución del comando con privilegios de superusuario.
- apt: El gestor de paquetes que estamos utilizando.
- install: Este subcomando se utiliza para instalar un nuevo paquete.
- isc-dhcp-server: Es el nombre del paquete que contiene el servidor DHCP que queremos instalar. Este software permitirá a la Raspberry Pi actuar como un servidor DHCP en la red.

### Configuración de DHCP en Packet Tracer
Abrimos el archivo de configuración de DHCP:
```bash
sudo nano /etc/dhcp/dhcpd.conf
```
- sudo: Permite editar el archivo con privilegios de superusuario.
- nano: Es un editor de texto de línea de comandos que permite modificar archivos.
- /etc/dhcp/dhcpd.conf: Es la ruta al archivo de configuración del servidor DHCP, donde se definen las subredes y las configuraciones que el servidor DHCP usará.

![imager](/imagenes/Sudo_nano.jpeg)

Agregamos la siguiente configuración para definir la subred:

```bash
subnet 192.168.5.0 netmask 255.255.255.0 {
    range 192.168.5.10 192.168.5.50;
    option routers 192.168.5.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
}
```
- subnet 192.168.5.0 netmask 255.255.255.0: Define una subred (en este caso, 192.168.5.0) con una máscara de red que indica que la subred tiene hasta 254 direcciones IP disponibles (de 192.168.5.1 a 192.168.5.254).
- range 192.168.5.10 192.168.5.50: Especifica el rango de direcciones IP que el servidor DHCP puede asignar a los clientes (de 192.168.5.10 a 192.168.5.50).
- option routers 192.168.5.1: Define la dirección IP del router (puerta de enlace) que los clientes usarán para acceder a otras redes.
- option domain-name-servers 8.8.8.8, 8.8.4.4: Especifica las direcciones IP de los servidores DNS que los clientes usarán para resolver nombres de dominio.

![imager](/imagenes/esquema_corregido)

### 4. Configuración de Interfaz en Packet Tracer
Configuramos la interfaz de red del servidor DHCP:

```bash
sudo nano /etc/default/isc-dhcp-server
```
- sudo: Permite la edición del archivo como superusuario.
- nano: El editor de texto utilizado para modificar el archivo.
- /etc/default/isc-dhcp-server: Archivo de configuración que contiene la configuración por defecto del servidor DHCP.

Agregamos eth0 en la variable INTERFACESv4:

```bash
INTERFACESv4="eth0"
```
- INTERFACESv4: Esta variable define las interfaces de red que el servidor DHCP escuchará para las solicitudes de DHCP.
- "eth0": Especifica que la interfaz de red a usar es eth0, que generalmente representa la conexión de red Ethernet en la Raspberry Pi.

![imager](/imagenes/ConfigInterfazDHCP.jpeg)

### 5. Configuración de Red Estática en Packet Tracer
Asignamos una IP estática a eth0 en el archivo de configuración de red:

```bash
sudo nano /etc/dhcpcd.conf
```
- sudo: Permite editar el archivo con privilegios de superusuario.
- nano: El editor de texto utilizado.
- /etc/dhcpcd.conf: Archivo de configuración que gestiona las configuraciones de red de la Raspberry Pi.

Añadimos las siguientes líneas:

```bash
interface eth0
static ip_address=192.168.5.1/24
static routers=192.168.5.1
static domain_name_servers=8.8.8.8 8.8.4.4
```
- interface eth0: Indica que las configuraciones que siguen se aplican a la interfaz eth0.
- static ip_address=192.168.5.1/24: Define la dirección IP estática que se asignará a la interfaz eth0, donde /24 indica la máscara de subred (255.255.255.0).
- static routers=192.168.5.1: Especifica la dirección IP del router, que en este caso es la misma que la IP estática asignada a la Raspberry Pi.
- static domain_name_servers=8.8.8.8 8.8.4.4: Define los servidores DNS que se utilizarán, en este caso, los servidores públicos de Google.

![imager](/imagenes/ConfigRedEstaticaDHCP.jpeg)

### 6. Reinicio y Comprobación del Servidor DHCP
Reiniciamos el servidor DHCP y el Raspberry para aplicar la configuración:

```bash
sudo systemctl restart isc-dhcp-server
sudo reboot
```
- sudo: Permite ejecutar el comando con privilegios de superusuario.
- systemctl: Herramienta para controlar el sistema y los servicios en Linux.
- restart: Indica que se debe reiniciar el servicio.
- isc-dhcp-server: Es el nombre del servicio DHCP que estamos reiniciando para aplicar los cambios de configuración.
------
- sudo: Permite reiniciar el sistema con privilegios de superusuario.
- reboot: Comando que reinicia la Raspberry Pi.

Para verificar que el servidor DHCP esté funcionando correctamente, utilizamos:

```bash
sudo journalctl -xe | grep dhcp
```
- sudo: Permite ejecutar el comando con privilegios de superusuario.
- journalctl: Herramienta que permite consultar los registros del sistema.
- -xe: Opciones para mostrar los registros en un formato más detallado.
- |: Este símbolo se utiliza para redirigir la salida del comando anterior al siguiente.
- grep dhcp: Filtra la salida para mostrar solo las líneas que contienen la palabra "dhcp", lo que permite verificar los registros relacionados con el servidor DHCP.

![imager](/imagenes/VerifDHCP.jpeg)
![imager](/imagenes/VerifConexionDHCP.jpeg)

Para verificar que funcione tambien hicimos un ping hacia mi computadora
![imager](/imagenes/Ping.jpeg)

## Conclusion
En este informe, configuramos la Raspberry Pi como servidor DHCP para administrar automáticamente las direcciones IP en una red LAN. Esta configuración facilita la administración de la red para conectar múltiples dispositivos en un entorno controlado, donde el servidor DHCP asigna IPs automáticamente a cada dispositivo conectado al switch de cada grupo.
