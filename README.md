# Evaluación 1: Servicios y paquetes

- Integrante: Nicolás Sepúlveda Falcón
  
- Curso: Administración de Sistemas



## Introducción
En este documento se describe el proceso de instalación, configuración y aprendizaje de varios servicios en un sistema operativo creado y desplegado por VirtualBox, utilizando Debian. Los servicios incluyen Apache y PHP para servir aplicaciones web, MySQL para gestión de base de datos, SSH para conexiones remotas seguras, Samba para compartir carpetas en red local y Node.js para despliegue de aplicaciones de JavaScript.

## Instalación y Configuración de Servicios
### Actualización del Sistema Operativo (Debian)
Antes de comenzar con la instalación de los servicios hay que actualizar el sistema operativo para asegurarse de que todos los paquetes estén en su última versión.
```bash
  sudo apt update
  sudo apt upgrade
```

### Instalación de Apache y PHP
#### Instalación de Apache
Apache es un servidor que permite servir aplicaciones web. Se instala con el siguiente comando:
```bash
  sudo apt install apache2
```

#### Instalación de PHP
PHP es un lenguaje de programación de servidor. Se instala con el siguiente comando:
```bash
  sudo apt install php
```

#### Instalación de Dependencias de PHP para Apache
Para realizar la integración de PHP con Apache es necesario instalar el siguiente módulo. Se instala con el siguiente comando:
```bash
  sudo apt install libapache2-mod-php -y
```

#### Verificación de las instalaciones de Apache y PHP
Se inicia el servicio de Apache y se verifica la versión de PHP para confirmar la correcta instalación. Se verifica con el siguiente comando:
```bash
  sudo systemctl start apache2
  sudo systemctl enable apache2
  sudo php -v
```

### Instalación de MySQL
#### Instalación del Servidor MySQL y configuración de Seguridad
MySQL es un sistema de gestión de base de datos relacional. Se instala con el siguiente comando y luego se asegura la instalación con: 
```bash
  sudo apt install mysql-server -y
  sudo mysql_secure_installation
```

#### Creación de usuarios en MySQL
Para crear usuarios con permisos especificos, se accede a la consola de MySQL
```bash
  sudo mysql -u root -p
```
Los usuarios creados incluyen un administrador (admin) con todos los permisos y un usuario lector (lector) con permisos limitados.

#### Referencia de la documentación para MySQL
La instalación y configuración de MySQL se realizó siguiendo unos pasos encontrados en [documentación de DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)

### Configuración del servidor SSH
#### Instalación de SSH
El servidor SSH permite conexiones remotas seguras al servidor. Se instala con el siguiente comando:
```bash
  sudo apt install openssh-server -y
```

#### Configuración de SSH para conexiones remotas
Primero se valida la correcta instalación de SSH y, posteriormente, se edita el archivo de configuración de SSH para permitir conexiones remotas con los siguientes comandos:
```bash
  sudo systemctl start ssh
  sudo systemctl enable ssh
  sudo nano /etc/ssh/sshd_config
```

Luego, se debe buscar las siguientes líneas de código y verificar que estén en 'yes' ambas opciones:
```bash
  PermitRootLogin yes
  PasswordAuthentication yes
```
#### Reinicio del servicio SSH
Después de realizar los cambios se debe reiniciar el servicio SSH para aplicar la configuración. Se reinicia con el siguiente comando:
```bash
  sudo systemctl restart ssh
```

### Instalación y configuración de Samba
#### Instalación de Samba
Samba permite compartir archivos y carpetas en una red local. Se instala con el siguiente comando:
```bash
  sudo apt install samba -y
```

#### Crear carpeta compartida
Se crea la carpeta compartida y es ubicada en home. Se utiliza el siguiente comando:
```bash
  sudo mkdir -p /svr/samba/compartida_publica
```
Luego, se otorgan permisos a la carpeta. Se utiliza el siguiente comando:
```bash
  sudo chmod 777 /svr/samba/compartida_publica
```
Finalmente, se hace un enlace simbólico en la home. Cabe destacar que 'nico' es el nombre de usuario. Se utiliza el siguiente comando:
```bash
  ln -s /svr/samba/compartida_publica /home/nico/compartida_publica
```




