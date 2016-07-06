---
author: Sergio Silva
description: Tutorial para instalar servidor apache en centos 7
keywords: apache, instalar servidor, instalar servidor apache, centos 7
title: Instalar servidor apache en centos 7
type: image
---

# Centos Install Apache

centos install apache Un servidor centos siempre lo puedes usar como un web server para mostrar páginas web con el servidor apache ya sea para mostrar algún sitio de la empresa para la que trabajes o para mostrar un blog personal, centos es un linux server que te puede ayudar tener estos servicios operando en 15 minutos, {{ hosting_name }} pero no es tan simple como escribir centos install apache, la verdad, sería bueno que se hiciera así pero no te preocupes aquí te digo los pasos de cómo instalar apache en centos 7 o 6.x.

## Centos Install Apache

Como te mencioné, en centos puedes instalar un web server y en general es un procesos sencillo que puedes hacer con unos cuantos pasos, pero quiero comentarte que en la versión 7 de Centos y Red Hat {{ hosting_name }} no funcionan los mismo comandos que en las versiones anteriores como Centos 6.5, por esto aquí te pongo las dos formas para que no tengas problemas servidor apache independientemente de cual versión de centos tengas.

Para instalar el servidor apache en centos 6.X o en Centos 7 puedes {{ var_x }} usar el siguiente comando:
~~~~
sudo yum install httpd
~~~~
Luego de descargar e instalar los paquetes lo que tienes que hacer es habilitar que el servicio arranque por default al encender el servidor y arrancar el servicio manualmente para poder luego verificar que funcione. Para hacer estos pasos ejecuta lo siguiente:

### Centos 6.X
~~~~
chkconfig httpd on
service httpd start
~~~~

### Centos 7
~~~~
systemctl enable httpd.service
systemctl start httpd.service
~~~~

Al momento de arrancar el servicio puedes verificar el si está funcionando correctamente al abrir un navegador e ir a la dirección IP del servidor, si has configurado la interfaz gráfica en Centos puedes abrir un navegador y teclear “localhost” o 127.0.0.1 en la URL y te mostrará una pagina como esta:
~~~~
probar centos install apache
~~~~

Si no tienes una interfaz gráfica puedes instalar un navegador web en texto como lynx, o puedes también verificar el estado del puerto 80 para saber si está escuchando, para realizar estas verificaciones puedes ver los siguientes ejemplos:
~~~~
yum install nmap
nmap localhost -p 80
~~~~

Recuerda que puedes ver la lista completa de instalación de servicios en centos al final del artículo: Como instalar centos como servidor

Espero que te sea de utilidad y puedas operar tu servidor web en centos.
