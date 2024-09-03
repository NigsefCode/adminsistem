# Evaluación 1: Servicios y paquetes

Administración de Sistemas
Integrante: Nicolás Sepúlveda Falcón


## Instalación
## Entrar modo administrador (sudo)
```bash
  su -
```

## Actualizar Debian
```bash
  apt update
  apt upgrade
```
## Apache y PHP
## Instalación Apache2
```bash
  apt install apache2
```

## Instalación PHP
```bash
  apt install php
```

## Instalación Dependencias PHP/Apache2
```bash
  apt install ibapache2-mod-php -y
```

## Probar instalaciones de PHP y Apache2
```bash
  systemctl start apache2
  systemctl enable apache2
  php -v
```
## MySQL
## Descargar paquete MySQL
```bash
  wget https://dev.mysql.com/get/mysql-apt-config_0.8.32-1_all.deb
```

## Instalar paquete MySQL
```bash
  apt install ./mysql-apt-config_0.8.32-1_all.deb
```
