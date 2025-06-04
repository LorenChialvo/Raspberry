# Instalación del Xorg en Raspberry Pi

## Índice
1. [Introducción](#introducción)
2. [Objetivo del Informe](#objetivo-del-informe)
3. [Desarrollo Teórico](#desarrollo-teorico)
   - [¿Qué es Xorg?](#qué-es-xorg)
   - [¿Cómo funciona el servidor gráfico en modo remoto?](#cómo-funciona-el-servidor-gráfico-en-modo-remoto)
4. [Pasos para Instalar Xorg](#pasos-para-instalar-xorg)
   - [Actualización del sistema](#actualización-del-sistema)
   - [Instalación de Xorg](#instalación-de-xorg)
   - [Configuración de Xorg para SSH](#configuración-de-xorg-para-ssh)
   - [Verificacion de funcionamiento](#verificación-de-funcionamiento)
5. [Conclusión](#conclusión)
6. [Volver al README](https://github.com/SyT-2024/tp-raspberry-grupo-5/blob/main/README.md)

---

## Introducción
En esta entrega, instalamos el servidor gráfico Xorg en la Raspberry Pi sin incluir un gestor de ventanas ni de escritorio. Esto nos permitirá ejecutar aplicaciones gráficas a través de SSH, delegando el procesamiento gráfico al cliente en lugar de cargar al servidor.

## Objetivo del Informe
El objetivo de este informe es detallar el proceso de instalación del servidor gráfico Xorg en la Raspberry Pi y cómo lo configuramos para ejecutar aplicaciones gráficas de forma remota a través de SSH. En este modo, la Raspberry Pi se encargará de enviar los datos al cliente, mientras que el procesamiento gráfico se realiza en el equipo remoto.

---

## Desarrollo Teórico

### ¿Qué es Xorg?
Xorg es el servidor gráfico estándar para sistemas Unix y Linux. Proporciona las bases para que las aplicaciones gráficas puedan comunicarse con los dispositivos de entrada y salida. Aunque normalmente se usa junto con un entorno de escritorio, en este proyecto solo instalamos Xorg para permitir que las aplicaciones gráficas se ejecuten sin un escritorio o un gestor de ventanas.

### ¿Cómo funciona el servidor gráfico en modo remoto?
Cuando conectamos una Raspberry Pi mediante SSH y ejecutamos aplicaciones gráficas, Xorg permite que las aplicaciones envíen sus gráficos al cliente (computadora remota). De este modo, todo el procesamiento gráfico se realiza en el cliente, no en la Raspberry Pi, lo que es ideal para aprovechar los recursos del cliente.

---

## Pasos para Instalar Xorg

### 1. Actualización del sistema
Antes de instalar cualquier paquete, debemos actualizar los repositorios y paquetes del sistema. Esto nos garantiza que estamos trabajando con las versiones más recientes.

Comandos que usamos:

```bash
sudo apt update
sudo apt upgrade
```

- `sudo`: Ejecuta el comando con privilegios de superusuario.
- `apt`: Utiliza el gestor de paquetes para obtener actualizaciones.
- `update`: Actualiza la lista de paquetes disponibles.
- `upgrade`: Instala las actualizaciones de los paquetes instalados.

### 2. Instalación de Xorg
Para instalar solo el servidor gráfico sin un gestor de ventanas ni de escritorio, utilizamos el siguiente comando:

```bash
sudo apt install xorg
```

- `xorg`: Es el paquete principal que incluye el servidor gráfico necesario para ejecutar aplicaciones gráficas.

### 3. Configuración de Xorg para SSH
Una vez instalado Xorg, debemos asegurarnos de que SSH permita el reenvío de gráficos. Esto se logra configurando el reenvío X11.

1. Editamos el archivo de configuración SSH en la Raspberry Pi:
```bash
sudo nano /etc/ssh/sshd_config
```

2. Nos aseguramos de que las siguientes líneas estén descomentadas o añadidas:
```plaintext
X11Forwarding yes
X11DisplayOffset 10
```

3. Reiniciamos el servicio SSH para aplicar los cambios:
```bash
sudo systemctl restart ssh
```

Ahora podemos conectarnos a la Raspberry Pi desde un cliente remoto utilizando el siguiente comando, que habilita el reenvío de gráficos:
```bash
ssh -X usuario@direccion_ip
```

- `-X`: Habilita el reenvío de gráficos X11.

### Verificación de Funcionamiento

1. Ejecutar xterm: 
   Una vez conectado, puedes ejecutar xterm para abrir un terminal gráfico:
   ```bash
   xterm   
   ```

2. Ejecutar otra aplicación gráfica: 
   Para probar el reenvío de X11, ejecuta:
   ```bash
   firefox-esr  
   ```

## Conclusión
En esta entrega, logramos instalar el servidor gráfico Xorg en la Raspberry Pi sin entorno de escritorio ni gestor de ventanas. Configuramos el sistema para que, a través de SSH, podamos ejecutar aplicaciones gráficas de forma remota, utilizando los recursos gráficos del cliente. Esto nos permitirá en futuras entregas instalar aplicaciones gráficas en la Raspberry Pi sin cargar su procesamiento.
