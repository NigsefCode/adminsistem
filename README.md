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

#### Crear una aplicación PHP
Se crea un archivo PHP en el directorio de documentos de apache, con el siguiente comando:
```bash
  sudo nano /var/www/html/index.php
```
Luego se escribe el contenido que se desea en el archivo 'index.php':

![image alt](https://github.com/NigsefCode/adminsistem/blob/067caf8924562fcb9ca2875021d78cb2e72a1f3f/PHP_Hola%20Mundo.png)

Por último, se accede a la aplicación creada, copiando la siguiente url en su navegador:
- http://localhost/index.php

Donde se debe reflejar lo escrito en el archivo:

![image alt](https://github.com/NigsefCode/adminsistem/blob/cbf7912c035226f095b6b0684eb0363caf68a307/PHP_Comprobacion%20Hola%20Mundo.png)

### Creación a la base de datos MySQL
Para la creación del script de PHP que demuestre una conexión a la base de datos MySQL, es necesario instalar:
```bash
  sudo apt install php-mysql
```
Luego, se crea el archivo donde se realizará la verificación:
```bash
  sudo nano /var/www/html/db_connect_mysql.php
```

### Instalación y configuración de MySQL
#### Instalación del Servidor MySQL y configuración de Seguridad
MySQL es un sistema de gestión de base de datos relacional. Se instala con el siguiente comando y luego se asegura la instalación con: 
```bash
  sudo apt install mysql-server -y
  sudo mysql_secure_installation
```
#### Configuración de la base de datos MySQL
Para la creación de una base de datos a modo de prueba, es necesario acceder a la consola MySQL
```bash
  sudo mysql -u root -p
```
Una vez en la consola, se puede utilizar la sintaxis de MySQL para crear la base de dates y crear un usuario administrador (admin) con todos los permisos y un usuario lector (lector) con permisos limitados:

Creación de la base de datos:

![image alt](https://github.com/NigsefCode/adminsistem/blob/8e8a4e96e3b54c7ceae84308da65d0c5fd8ff7df/MySQL_Crear%20Base%20de%20datos%20prueba.png)

Crear usuarios y permisos:

![image alt](https://github.com/NigsefCode/adminsistem/blob/fbf2862509383838d4f60c40af4476644fbbfd77/MySQL_Usuarios%20y%20Permisos.png)

Verificar usuarios creados:

![image alt](https://github.com/NigsefCode/adminsistem/blob/91fb7b6649f0e7e8686dcd303861e36e9a6d542b/MySQL_Verificar%20usuarios%20creados.png)


#### Referencia de la documentación para MySQL
La instalación y configuración de MySQL se realizó siguiendo unos pasos encontrados en [documentación de DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)

### Instalación y configuración de SSH
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
Para finalizar, después de realizar los cambios se debe reiniciar el servicio SSH para aplicar la configuración. Se reinicia con el siguiente comando:
```bash
  sudo systemctl restart ssh
```

#### Generar claves SSH en el host
Antes de generar claves en la computadora, se debe conocer la dirección IP de tu herramienta, si se utiliza alguna distribución Linux (Ubuntu, Debian), se puede conocer con el siguiente comando:
```bash
  nmcli -p device show
```
La dirección IP es la que se encuentra junto al elemento 'IP4.ADDRESS'

### Instalación y configuración de Samba
#### Instalación de Samba
Samba permite compartir archivos y carpetas en una red local. Se instala con el siguiente comando:
```bash
  sudo apt install samba -y
```

#### Crear carpeta compartida
Se crea la carpeta compartida. Se utiliza el siguiente comando:
```bash
  sudo mkdir -p /srv/samba/compartida_publica
```
Luego, se otorgan permisos a la carpeta. Se utiliza el siguiente comando:
```bash
  sudo chmod 777 /srv/samba/compartida_publica
```
Se edita el archivo de configuración de Samba, con:
```bash
  sudo nano /etc/samba/smb.conf
```
Se agrega la dirección al final del archivo para compartir carpeta:
```bash
  [compartida_publica]
   path = /home/nico/compartida
   available = yes
   valid users = nico
   read only = no
   browsable = yes
   public = yes
   writable = yes
   force user = nico
```
Se reinicia el servicio de Samba, con el siguiente comando:
```bash
  sudo systemctl restart smbd
```
Se hace un enlace simbólico en la home. Cabe destacar que 'nico' es el nombre de usuario. Se utiliza el siguiente comando:
```bash
  ln -s /srv/samba/compartida_publica /home/nico/compartida_publica
```
Se confirma si se completo el enlace simbólico en la home. Estando en home se debe utilizar el siguiente comando:
```bash
  ls
```

![image alt](https://github.com/NigsefCode/adminsistem/blob/2278af5fc6d7f9b98e1b0107ddd77eeb57ae3543/Samba_Carpeta%20compartida%20en%20Home.png)

Finalmente, se enlaza la carpeta compartida a la raíz del servidor web Apache
```bash
  sudo ln -s /home/nico/compartida_publica /var/www/html/compartida_publica
```
Para confirmar el enlace en el servidor web Apache se debe realizar una búsqueda de la carpeta en la raíz del servidor web Apache. Se utiliza el siguiente comando:
```bash
  ls /var/www/html
```

![image alt](https://github.com/NigsefCode/adminsistem/blob/8f346a45cd5819ccec11dfdd13bcd20e55e2d1c0/Samba_Carpeta%20Compartida%20en%20raiz%20Apache.png)

### Instalación de Node.js
#### Instalación de Node.js
Se instala Node.js  y se verifica su versión para comprobar la correcta instalación. Se utiliza los siguientes comandos:
```bash
  sudo apt install nodejs
  nodejs -v
```

#### Instalación de npm
Se instala el administrador de paquetes de Node.js, llamado npm. Se utiliza el siguiente comando:
```bash
  sudo apt install npm
```

### Verificación de servicios
#### Verificación Final
Se verifica si los servicios estan funcionando correctamente con el siguiente comando:
```bash
  sudo systemctl status apache2 mysql ssh smbd
```



