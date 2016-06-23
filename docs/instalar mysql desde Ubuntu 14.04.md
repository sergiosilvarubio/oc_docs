---
author: Sergio Silva
description: Tutorial para Instalar mysql desde Ubuntu 14.04
keywords: mysql,instalar mysql,instalar mysql ubuntu,ubuntu 14.04
title: Instalar mysql desde Ubuntu 14.04
---

# Instalar mysql desde Ubuntu 14.04

Para poder agregar MySQL en Ubuntu es necesario abrir una terminal (Ctrl + Shift + t) y escribir (o copiar y pegar) la siguiente linea:
~~~~
    sudo apt-get install mysql-server mysql-client
~~~~

Con lo cual, después de haber ingresado la contraseña correctamente, procederá a instalar tanto el servidor como el cliente de MySQL. Seguido de esto se mostrara la siguiente interfaz en la terminal:
Interfaz de Ingreso de Password MySQL

## Interfaz de Ingreso de Password MySQL
### Interfaz de confirmacion de la contraseña del usuario root

Esta interfaz nos pide una contraseña para la cuenta de root ( la cuenta principal del servidor MySQL). Asignamos la contraseña y la confirmación y con esto terminamos la instalación de MySQL.

Ahora, para acceder al cliente de MySQL de terminal, lo único que tenemos que hacer es ingresar en la terminal:
~~~~
    mysql -uroot -p
~~~~
Seguido de esto nos solicitara la contraseña y, habiéndola ingresado correctamente nos mostrara la siguiente interfaz:
Cliente MySQL en terminal

## Cliente MySQL en terminal

### EXTRA:

### INSTALACION DE  DRIVER MySQL para Python:

Como ultimamente realizo algunos proyectos en Django, me sera necesario instalar el driver y ya que es una instalación nueva de Ubuntu procederé a instalar pip (un gestor de paquetes para Python)
~~~~
    sudo apt-get install pip
~~~~
Teniendo instalado pip, lo unico que queda es instalar el driver correspondiente para MySQL, lo cual se realiza con el siguiente comando:
~~~~
    sudo apt-get install python-mysqldb
~~~~
Me gusta mucho probar las librerias con bpython (sudo apt-get install bpython o pip install bpython), ya que tiene auto-complete y coloreado automatico de codigo, entre otras funciones geniales:
