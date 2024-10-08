# Evaluación 1: Servicios y paquetes

- Integrante: Nicolás Sepúlveda Falcón
  
- Curso: Administración de Sistemas



## Introducción
Este documento tiene como objetivo orientar y describir el proceso de instalación, configuración y aprendizaje de varios servicios en un sistema operativo creado y desplegado por VirtualBox. Se utilizará el sistema operativo Debian, donde los servicios incluyen Apache y PHP para servir aplicaciones web, MySQL para gestión de base de datos, SSH para conexiones remotas seguras, Samba para compartir carpetas en red local y Node.js para despliegue de aplicaciones de JavaScript.


## Instalación y Configuración de Servicios
### Actualización del Sistema Operativo (Debian)
Antes de comenzar con la instalación de los servicios hay que actualizar el sistema operativo para asegurarse de que todos los paquetes estén en su última versión.
```bash
  sudo apt update
  sudo apt upgrade
```

### Instalación y configuración de MySQL
#### Instalación del Servidor MySQL y configuración de Seguridad
MySQL es un sistema de gestión de base de datos relacional. Primero se descarga el 'dev' de la página oficial de mysql, se puede hacer desde la terminal con el siguiente comando:
```bash
  wget https://dev.mysql.com/get/mysql-apt-config_0.8.32-1_all.deb
```
Posteriormente se instala el archivo descargado:
```bash
  sudo apt install ./mysql-apt-config_0.8.32-1_all.deb
```
Luego se actualiza el sistema para la adquisición completa de los paquetes de MySQL y se instala con el siguiente comando y se asegura la instalación con: 
```bash
  sudo apt install mysql-server -y
  sudo mysql_secure_installation
```
#### Configuración de la base de datos MySQL
Para la creación de una base de datos a modo de prueba, es necesario acceder a la consola MySQL
```bash
  sudo mysql -u root -p
```
Una vez en la consola, se puede utilizar la sintaxis de MySQL para crear la base de datos y crear un usuario administrador (admin) con todos los permisos y un usuario lector (lector) con permisos limitados:

Creación de la base de datos:

![image alt](https://github.com/NigsefCode/adminsistem/blob/8e8a4e96e3b54c7ceae84308da65d0c5fd8ff7df/MySQL_Crear%20Base%20de%20datos%20prueba.png)

Crear usuarios y permisos:

![image alt](https://github.com/NigsefCode/adminsistem/blob/fbf2862509383838d4f60c40af4476644fbbfd77/MySQL_Usuarios%20y%20Permisos.png)

Verificar usuarios creados:

![image alt](https://github.com/NigsefCode/adminsistem/blob/91fb7b6649f0e7e8686dcd303861e36e9a6d542b/MySQL_Verificar%20usuarios%20creados.png)

Sin embargo, para que los usuarios funcionen en el host, es necesario realizar algunas modificaciones para que los usuarios se puedan conectar desde el host al servicio de la máquina virtual en MySQL:
Se ingresa al panel de mysql:
```bash
  sudo mysql -u root -p
```

Y se ejecutan las siguientes líneas de código:
```bash
  RENAME USER 'admin'@'localhost' TO 'admin'@'%';
  RENAME USER 'lector'@'localhost' TO 'lector'@'%';
  FLUSH PRIVILEGES;
```
Además, se configura el Firewall para permitir conexiones en el puerto 3306:
```bash
  sudo apt install ufw
  sudo ufw allow 3306
```
Y al intentar entrar desde PowerShell en Windows:
```bash
  mysql -h ip_maquina_virtual -u admin -p prueba
  mysql -h ip_maquina_virtual -u lector -p prueba
```

![image alt](https://github.com/NigsefCode/adminsistem/blob/f85f3a7e7703eac6b0cf88063d75a32b9c0134b2/MySQL_Base%20de%20datos%20en%20Host.png)

#### Referencia de la documentación para MySQL
La instalación y configuración de MySQL se realizó siguiendo unos pasos encontrados en [documentación de DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)

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

Para verificar la aplicación en el navegador del host:
- http://ip_maquina_virtual/index.php

![image alt](https://github.com/NigsefCode/adminsistem/blob/aad13392d83257d85bbc1ad8eb39a6a0f1e35351/PHP_HolaMundo_Host.png)

#### Verificación de conexión a la base de datos MySQL
Para la creación del script de PHP que demuestre una conexión a la base de datos MySQL, es necesario instalar:
```bash
  sudo apt install php-mysql
```
Luego, se crea el archivo donde se realizará la verificación:
```bash
  sudo nano /var/www/html/db_connect_mysql.php
```
Para, posteriormente, crear la verificación en el archivo creado. Observar el siguiente ejemplo:

![image alt](https://github.com/NigsefCode/adminsistem/blob/2295bad50fc2383966c4034fa84bdc0f38a4a511/PHP_Crear%20Verificacion%20en%20PHP%20de%20MySQL.png)

Se puede verificar la conexión a la base de datos en el navegador:
- http://localhost/db_connect_mysql.php

Donde se debe reflejar la conexion de la siguiente manera:

![image alt](https://github.com/NigsefCode/adminsistem/blob/a26f67464ee52d9e63ec71fb146f4766f309e566/PHP_Compracion%20de%20verificacion%20BD.png)

Para confirmar si la conexión es exitosa desde el host para los usuarios administrador y lector se debe entrar desde el navegador y, para este ejemplo, se modificó el codigo para imprimir la conexión con respecto al usuario creado:
- http://ip_maquina_virtual/db_connect_mysql.php

Administrador:

![image alt](https://github.com/NigsefCode/adminsistem/blob/7f908bc666dd00faf43ca0a4dd300b939af7db70/PHP_AdminDatos.png)

![image alt](https://github.com/NigsefCode/adminsistem/blob/7d4166cac746563ecc1d6822fb808a5c05366fc2/PHP_Conexion%20Admin_Host.png)

Lector:

![image alt](https://github.com/NigsefCode/adminsistem/blob/e6543e00fd2e584c95543e2e20c99a237259c58a/PHP_LectorDatos.png)

![image alt](https://github.com/NigsefCode/adminsistem/blob/a4387db2c33fb86c8d21f181078e321690a9443a/PHP_Conexion%20Lector_Host.png)

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
La dirección IP es la que se encuentra junto al elemento 'IP4.ADDRESS'.

Para generar claves SSH de tamaño estándar en la máquina host, se puede hacer con el siguiente comando:
```bash
  ssh-keygen
```
En el caso de querer personalizar el tamaño de la clave en bits puede hacerse de la siguiente manera:
```bash
  ssh-keygen -t rsa -b 4096
```
Para este ejemplo se utilizó el tamaño estándar. Ahora se copia la clave pública a la máquina virtual con el siguiente comando:
```bash
  ssh-copy-id usuario@ip_máquina_virtual
```
En caso de estar en Windows 10/11 en PowerShell y no funcione el comando, se puede hacer de forma manual de la siguiente forma:
```bash
  cat ~/.ssh/id_rsa.pub | ssh usuario@ip_maquina_virtual  "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
Luego, entrar a la máquina virtual desde la terminal de PowerShell:
```bash
  ssh usuario@ip_maquina_virtual
```
Y se otorgan permisos de acceso:
```bash
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/authorized_keys
```
Se puede verificar volviendo a antrar a la máquina virtual con
```bash
  ssh usuario@ip_maquina_virtual
```
Un ejemplo del acceso es:

![image alt](https://github.com/NigsefCode/adminsistem/blob/48c5e0d91efe375c0342c94ef37392f9f731f949/SSH_Conexion%20de%20Host%20a%20Debian_Filtrado.png)

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
   path = /home/usuario/compartida
   available = yes
   guest ok = yes
   create mask = 0755
   directory mask = 0755
   valid users = usuario
   read only = no
   browseable = yes
   public = yes
   writable = yes
   force user = usuario
```
Se reinicia el servicio de Samba, con el siguiente comando:
```bash
  sudo systemctl restart smbd
```
Se hace un enlace simbólico en la home. Se utiliza el siguiente comando:
```bash
  ln -s /srv/samba/compartida_publica /home/usuario/compartida_publica
```
Se confirma si se completó el enlace simbólico en la home. Estando en home se debe utilizar el siguiente comando:
```bash
  ls
```

![image alt](https://github.com/NigsefCode/adminsistem/blob/2278af5fc6d7f9b98e1b0107ddd77eeb57ae3543/Samba_Carpeta%20compartida%20en%20Home.png)

Finalmente, se enlaza la carpeta compartida a la raíz del servidor web Apache
```bash
  sudo ln -s /home/usuario/compartida_publica /var/www/html/compartida_publica
```
Para confirmar el enlace en el servidor web Apache se debe realizar una búsqueda de la carpeta en la raíz del servidor web Apache. Se utiliza el siguiente comando:
```bash
  ls /var/www/html
```

![image alt](https://github.com/NigsefCode/adminsistem/blob/8f346a45cd5819ccec11dfdd13bcd20e55e2d1c0/Samba_Carpeta%20Compartida%20en%20raiz%20Apache.png)

Para demostrar el acceso en la carpeta compartida, se necesita instalar:
```bash
  sudo apt install smbclient
```
Luego, se crea una constraseña con el usuario:
```bash
  sudo smbpasswd -a usuario
```
Para terminar, se realiza la conexión remota ingresando el siguiente comando:
```bash
  smbclient //localhost/compartida_publica -U usuario
```
Se le pedirá la contraseña creada anteriormente y podra crear, eliminar o ver carpetas, entre otras cosas. Se puede observar en el siguiente ejemplo:

![image alt](https://github.com/NigsefCode/adminsistem/blob/3af8d5933a4bb5ab7134f1a35a5bc7b317a09556/SAMBA_Acceder%20a%20la%20carpeta%20compartida%20desde%20localhost.png)

En caso de que quiera ingresar desde otro dispositivo, puede utilizar el mismo comando pero cambiando la ip (se requiere conocer la contraseña del usuario):
```bash
  smbclient //ip_maquina_virtual/compartida_publica -U usuario
```

### Instalación y Configuración de Node.js
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

#### Configuración de Node.js
Se crea una carpeta en la home, donde se quiera trabajar, como por ejemplo:
```bash
  mkdir appsnode
```
Luego se entra a la carpeta y se genera un archivo js de la siguiente manera:
```bash
  cd appsnode
  nano app.js
```
Posteriormente se crea la aplicación Node.js como un Hola Node:
```bash
  const http = require('http');
  const port = 3000;
  
  const requestHandler = (request, response) => {
      response.end('Hello, Node.js!');
  };
  
  const server = http.createServer(requestHandler);
  
  server.listen(port, () => {
      console.log(`Server is running on http://localhost:${port}`);
  });
```
Se guarda el archivo y se ejecuta:
```bash
  node app.js
```
Para comprobar si el acceso de la aplicación Node.js funciona desde el navegador del host en el puerto 3000, se utiliza el siguiente url:
- http://ip_maquina_virtual:3000/

Donde se debería mostrar de la siguiente manera:

![image alt](https://github.com/NigsefCode/adminsistem/blob/686f4a0e655cf1e841fb63a3f449265a4bb05f82/NODEJS_App%20en%20Host%20nav.png)

### Verificación de servicios
#### Verificación Final
Se verifica si los servicios estan funcionando correctamente con el siguiente comando:
```bash
  sudo systemctl status apache2 mysql ssh smbd
```

### Logs de Comandos
Hubo un problema en el almacenaje de comandos, donde únicamente se muestran los comandos que ejecuté después de encender la máquina virtual. En otras palabras, todos los comandos utilizados antes de apagar la máquina virtual se perdieron. Desconozco el motivo, sin embargo, detallé cada paso y cada linea de código utilizado a lo largo del Readme. De igual manera, se subio un archivo [history.log](https://github.com/NigsefCode/adminsistem/blob/08c8e23921825fa26d97451d75c315f114b1c03f/history.log)

### Errores comunes y soluciones.
- Hubo un problema en MySQL sobre la instalación con apt, el sistema operativo no reconocia el comando ni la librería. Por ello, se tuvo que descargar el paquete dev, que al actualizar el sistema operativo, una vez instalado el paquete, reconocía el apt de las librerías de mysql. Además, hubo un problema al verificar usuarios en Windows 11, por ende se tuvo que modificar los usuarios para que puedan conectarse no solo en el localhost.
- Errores de sintaxis al crear el script de PHP sobre la conexión de MySQL. Para solucionarlo se buscó documentación y videos actualizados que permitan generar el script correctamente.
- Problemas al estar en Windows 10/11 y usar PowerShell, ya que hubieron comandos que no funcionaban y se tuvo que buscar documentación sobre comandos alternativos. Sin embargo, para el trabajo, en el caso de ssh, se hizo de forma manual las configuraciones correspondientes.
- En Samba, se tuvo que configurar los permisos de la carpeta compartida, en donde se tuvo que investigar mucho para configurarlos y reconocer lo que hace cada línea de código correspondiente.



