# Internet - DNS

## Índice
1. [Introducción](#introducción)
2. [Objetivo del Informe](#objetivo-del-informe)
3. [Desarrollo Teórico](#desarrollo-teórico)
    - [¿Qué es el DNS?](#qué-es-el-dns)
4. [Explicación de Comandos](#explicación-de-comandos)
   - [Instalación de Iptables](#instalación-de-iptables)
   - [Conexión a la Red Inalámbrica del WAP](#conexión-a-la-red-inalámbrica-del-wap)
   - [Configuración de Interfaces de Red](#configuración-de-interfaces-de-red)
   - [Activación del Enrutamiento de Tráfico](#activación-del-enrutamiento-de-tráfico)
   - [Configuración de NAT para Redireccionamiento de Tráfico](#configuración-de-nat-para-redireccionamiento-de-tráfico)
   - [Configuración del Servidor DNS](#configuración-del-servidor-dns)
5. [Conclusión](#conclusión)
6. [Volver al README](#link-del-readme)

## Introducción
En este informe, se describe cómo configurar una Raspberry Pi para que funcione como un gateway en una red LAN, permitiendo acceso a Internet a través de una conexión Wi-Fi establecida con un Wireless Access Point (WAP) o un hotspot móvil. Se mantendrá activa la configuración de DHCP previamente establecida para facilitar la asignación de direcciones IP en la red local.

## Objetivo del Informe
El propósito de este informe es proporcionar los pasos detallados para configurar la Raspberry Pi como una puerta de enlace en una red LAN, permitiendo que los dispositivos conectados puedan acceder a Internet a través de una conexión Wi-Fi o hotspot.

## Desarrollo Teórico

### ¿Qué es el DNS?
El Sistema de Nombres de Dominio (DNS, por sus siglas en inglés) es un sistema que permite traducir nombres de dominio (por ejemplo, www.example.com) a direcciones IP. Estas direcciones son fundamentales para que los dispositivos de una red puedan localizar y conectarse a servidores específicos en Internet.

### Instalación de Iptables
Para instalar iptables, la herramienta de configuración de firewall en Linux, usa los siguientes comandos:

   ```bash
sudo apt update
   ```

sudo: Ejecuta el comando como superusuario, necesario para modificar configuraciones del sistema.
apt: Es el gestor de paquetes en sistemas basados en Debian, como Ubuntu y Raspberry Pi OS.
update: Actualiza la lista de paquetes disponibles y sus versiones en los repositorios.

   ```bash
sudo apt install iptables
   ```

sudo: Ejecuta el comando con privilegios de superusuario (administrador). Esto es necesario para realizar cambios en el sistema, como instalar paquetes o modificar configuraciones.

apt: Es el gestor de paquetes en distribuciones basadas en Debian (como Ubuntu y Raspberry Pi OS). Se utiliza para instalar, actualizar y gestionar software en el sistema.

install: Indica que queremos instalar un paquete en el sistema.

iptables: Es el nombre de la herramienta que vamos a instalar para gestionar las reglas de firewall.

### Conexión a la Red Inalámbrica del WAP
Para conectar la Raspberry Pi a la red Wi-Fi, edita el archivo wpa_supplicant.conf:

   ```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
   ```

sudo: Se utiliza para ejecutar el comando con permisos de superusuario, permitiendo modificar 
archivos del sistema que normalmente no son accesibles por un usuario estándar.

nano: Es un editor de texto en terminal que permite abrir y editar archivos directamente desde la línea de comandos.

/etc/wpa_supplicant/wpa_supplicant.conf: Es la ruta del archivo de configuración para wpa_supplicant, 
el servicio encargado de gestionar la conexión a redes Wi-Fi en la Raspberry Pi o cualquier sistema Linux. 
En este archivo se configuran los detalles de las redes inalámbricas, como el SSID y la contraseña.


Agrega los detalles de la red:

   ```bash
network={
    ssid="Grupo5"
    psk="grupo5-2024"
    key_mgmt=WPA-PSK
}
   ```

network={ ... }: Define un bloque de configuración de red en el archivo wpa_supplicant.conf, donde se especifican los detalles necesarios 
para conectarse a una red Wi-Fi. Este bloque puede contener múltiples parámetros para identificar y acceder a la red deseada.

ssid="Grupo5": Especifica el SSID (Service Set Identifier) de la red Wi-Fi a la que se quiere conectar la Raspberry Pi. En este caso, 
el nombre de la red es "Carlitoss". El SSID es el nombre que identifica públicamente a una red inalámbrica.

psk="grupo5-2024": Indica la contraseña de la red Wi-Fi para conectarse de forma segura. El parámetro psk contiene la clave de acceso de la 
red especificada en el SSID. En este caso, la contraseña es "grupo5-2024".

key_mgmt=WPA-PSK: Define el tipo de gestión de claves utilizado por la red Wi-Fi, en este caso WPA-PSK`(Wi-Fi Protected Access - Pre-Shared Key). 
Este método de autenticación utiliza una clave compartida para autorizar el acceso a la red de forma segura.

### Configuración de Interfaces de Red
Para configurar la red, establece una IP estática para eth0 y configura wlan0 para obtener una IP de la red Wi-Fi:
   ```bash
sudo nano /etc/network/interfaces

   ```
sudo: Ejecuta el comando con permisos de superusuario para modificar archivos del sistema.

nano: Abre el archivo especificado en el editor de texto nano.

/etc/network/interfaces: Es el archivo de configuración de las interfaces de red en Linux. 
Aquí se definen los parámetros de conexión para interfaces como eth0 (cableada) y wlan0 (inalámbrica), 
incluyendo dirección IP, máscara de subred y puerta de enlace.

   ```bash
auto eth0
iface eth0 inet static
    address 192.168.5.1
    netmask 255.255.255.0
    gateway 192.168.5.1

auto wlan0
iface wlan0 inet dhcp
    wpa-ssid "Grupo5"
    wpa-psk "grupo5-2024"
   ```

auto eth0: Indica que la interfaz eth0 se activará automáticamente al arrancar el sistema.

iface eth0 inet static: Configura la interfaz eth0 con una dirección IP estática en lugar de recibirla por DHCP.

address 192.168.5.1: Define la dirección IP de la interfaz eth0 en la red local (LAN). En este caso, la IP es 192.168.5.1, lo 
que convierte a la Raspberry Pi en la puerta de enlace de la LAN.

netmask 255.255.255.0: Especifica la máscara de subred para eth0, definiendo el rango de direcciones IP válidas en la red local. 
La máscara 255.255.255.0 permite 254 direcciones en la misma red.

gateway 192.168.5.1: Define la puerta de enlace de la red local. Aquí, 192.168.5.1 es la propia Raspberry Pi, que actuará 
como puerta de enlace para los dispositivos conectados a la LAN, dirigiendo su tráfico hacia la red externa.

auto wlan0: Indica que la interfaz wlan0 se activará automáticamente al arrancar el sistema.

iface wlan0 inet dhcp: Configura la interfaz wlan0 para obtener una dirección IP de forma dinámica mediante DHCP, asignada por 
el punto de acceso inalámbrico o el hotspot.

wpa-ssid "Grupo5": Especifica el SSID de la red Wi-Fi a la que wlan0 intentará conectarse. En este caso, la red se llama "Grupo5".

wpa-psk "grupo5-2024": Define la clave de seguridad de la red Wi-Fi para el SSID especificado, permitiendo que wlan0 se conecte de forma segura.

### Activación del Enrutamiento de Tráfico
Para habilitar el reenvío de tráfico entre interfaces de red:

   ```bash
sudo nano /etc/sysctl.conf
   ```

sudo: Ejecuta el comando con permisos de superusuario, permitiendo la modificación 
de archivos del sistema que requieren privilegios elevados.

nano: Abre el editor de texto nano en la terminal para visualizar o editar el contenido de un archivo.

/etc/sysctl.conf: Es el archivo de configuración del sistema que controla varios parámetros del núcleo 
de Linux. Aquí se puede habilitar o deshabilitar opciones, como el reenvío de paquetes IP 
(configuración importante para que la Raspberry Pi actúe como puerta de enlace entre la red LAN y la red Wi-Fi).

Descomenta la línea:

   ```bash
net.ipv4.ip_forward=1
   ```

net.ipv4.ip_forward: Es un parámetro de configuración del núcleo (kernel) de Linux que controla el reenvío de paquetes IP. 
Activar el reenvío permite que el sistema envíe paquetes de una interfaz de red a otra, actuando como un router o puerta de 
enlace para el tráfico de red.

=1: Establece el valor de ip_forward en 1, lo cual habilita el reenvío de paquetes IP. Esto permite que los paquetes
que llegan a una interfaz de red (por ejemplo, eth0) puedan ser reenviados a otra interfaz (como wlan0), y viceversa.

Aplica los cambios:

   ```bash
sudo sysctl -p
   ```

sudo: Ejecuta el comando con permisos de superusuario.

sysctl: Es una herramienta para ver o modificar parámetros del núcleo en tiempo real.

-p: Este parámetro aplica los cambios en la configuración de sysctl.conf, donde 
se especifican parámetros del núcleo, como el reenvío de paquetes IP.

Esto permite que la Raspberry Pi reenvíe paquetes entre eth0 y wlan0, habilitando el tráfico entre ambas interfaces.

### Configuración de NAT para Redireccionamiento de Tráfico
Configura el redireccionamiento para que los dispositivos de la LAN puedan acceder a Internet a través de wlan0:

   ```bash
sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
   ```

sudo: Ejecuta el comando con permisos de superusuario.

iptables: Es una herramienta para configurar las reglas del firewall en Linux.

-t nat: Especifica que se trabajará en la tabla nat (Network Address Translation), que se utiliza para traducir direcciones de red.

-A POSTROUTING: Añade una regla a la cadena POSTROUTING, que se aplica después de que el paquete ha sido enrutado.

-o wlan0: Define que la regla se aplica a paquetes salientes a través de la interfaz wlan0.

-j MASQUERADE: Utiliza MASQUERADE, que permite a los dispositivos de la red LAN usar la dirección IP de wlan0 para acceder a Internet.

Guarda la configuración para que se aplique tras reiniciar:

```bash
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```

sudo: Ejecuta el comando con permisos de superusuario.

sh -c: Ejecuta el comando entre comillas en una subshell.

iptables-save: Guarda las reglas actuales de iptables en formato legible para su restauración posterior.

> /etc/iptables.ipv4.nat: Redirige la salida de iptables-save al archivo especificado para que las reglas 
puedan cargarse automáticamente al iniciar el sistema.

Edita /etc/rc.local para restaurar las reglas al iniciar:

```bash
sudo nano /etc/rc.local
```

sudo: Ejecuta el comando con permisos de superusuario para poder modificar archivos protegidos del sistema.

nano: Abre el editor de texto nano en la terminal, permitiendo ver o editar el contenido del archivo especificado.

/etc/rc.local: Es un archivo de inicio en sistemas Linux que permite ejecutar comandos o scripts al inicio del sistema. 
Las instrucciones colocadas en este archivo se ejecutarán automáticamente cada vez que el sistema arranque.

Añade antes de exit 0:

```bash
iptables-restore < /etc/iptables.ipv4.nat
```

iptables-restore: Es un comando utilizado para restaurar las reglas de iptables desde 
un archivo de configuración guardado. Este comando carga las reglas previamente definidas 
para el firewall y NAT en el sistema.

< /etc/iptables.ipv4.nat: Especifica el archivo de origen desde el cual se restaurarán las reglas de iptables. 
En este caso, /etc/iptables.ipv4.nat contiene las configuraciones de NAT necesarias para permitir que la red LAN 
acceda a Internet a través de la interfaz Wi-Fi.

### Configuración del Servidor DNS
Para que la Raspberry Pi use servidores DNS válidos, configura el archivo resolv.conf:

```bash
nameserver 8.8.8.8
nameserver 8.8.4.4
```

nameserver 8.8.8.8: Especifica que el primer servidor DNS que se utilizará para 
resolver nombres de dominio es el servidor de Google con la dirección IP 8.8.8.8. 
El propósito de un servidor DNS es convertir los nombres de dominio, como www.google.com, 
en direcciones IP que las computadoras puedan entender para hacer la conexión de red.

nameserver 8.8.4.4: Define un servidor DNS secundario, también de Google, con la IP 8.8.4.4. 
Si el servidor primario (8.8.8.8) no responde o no está disponible, la Raspberry Pi utilizará 
este servidor para resolver nombres de dominio, garantizando así que las consultas DNS se realicen de manera continua.

Estos servidores DNS de Google permitirán que los dispositivos de la red local puedan resolver nombres de dominio.

### Conclusión
Con esta configuración, la Raspberry Pi actúa como una puerta de enlace que conecta una red LAN a una red externa, como una red Wi-Fi de un WAP. Gracias a la configuración de NAT y los ajustes en el servidor DNS, los dispositivos en la LAN podrán navegar por Internet, cumpliendo así con los objetivos establecidos.
