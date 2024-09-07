# Evaluación 1: Servicios y paquetes

Administración de Sistemas
Integrante: Nicolás Sepúlveda Falcón


## Instalación


## Actualizar Debian
```bash
  sudo apt update
  sudo apt upgrade
```
## Apache y PHP
## Instalación Apache2
```bash
  sudo apt install apache2
```

## Instalación PHP
```bash
  sudo apt install php
```

## Instalación Dependencias PHP/Apache2
```bash
  sudo apt install libapache2-mod-php -y
```

## Probar instalaciones de Apache2 y PHP
```bash
  sudo systemctl start apache2
  sudo systemctl enable apache2
  sudo php -v
```

## MySQL
## Instalar MySQL Server y asegurar instalación
```bash
  sudo apt install mysql-server -y
  sudo mysql_secure_installation
```

## Ingreso a Consola MySQL para crear usuarios
```bash
  sudo mysql -u root -p
```

## Web sacada la documentación de instalación MySQL
```bash
  https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04
```

## SSH
## Instalar SSH
```bash
  sudo apt install openssh-server -y
```

## Probar installacion de SSH y permitir conexiones remotas
```bash
  sudo systemctl start ssh
  sudo systemctl enable ssh
  sudo nano /etc/ssh/sshd_config
```

## Reiniciar SSH
```bash
  sudo systemctl restart ssh
```

## Samba
## Instalar Samba
```bash
  sudo apt install samba -y
```





