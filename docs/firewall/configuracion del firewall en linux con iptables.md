---
author: Sergio Silva
description: Tutorial para configurar del firewall en linux con iptables
keywords: configurar,firewall,linux,iptables
title: Configuración firewall en linux con iptables
---

Un firewall es un dispositivo, ya sea software o hardware, que filtra todo el tráfico de red. El sistema operativo Linux dispone de un firewall llamado IPtables.

Iptables es un firewall incluido en el kernel de Linux desde la versión 2.4 que está incluido en el sistema operativo. Es un firewall basado en reglas, su funcionamiento se basa en aplicar reglas que el mismo firewall ejecute. Estas IPtables también se encuentran en los firmwares basados en Linux y por supuesto, los dispositivos Android.

El uso de IPtables es bastante complejo, por lo que vamos a hacer un vistazo general sobre sus opciones:
Para Iniciar/Parar/Reiniciar Iptables debemos ejecutar estos comandos:
~~~~
    sudo service iptables start
    sudo service iptables stop
    sudo service iptables restart
~~~~
Los principales comandos de IPtables son los siguientes (argumentos de una orden):
~~~~
    -A –append → agrega una regla a una cadena.
    -D –delete → borra una regla de una cadena especificada.
    -R –replace → reemplaza una regla.
    -I –insert → inserta una regla en lugar de una cadena.
    -L –list → muestra las reglas que le pasamos como argumento.
    -F –flush → borra todas las reglas de una cadena.
    -Z –zero → pone a cero todos los contadores de una cadena.
    -N –new-chain → permite al usuario crear su propia cadena.
    -X –delete-chain → borra la cadena especificada.
    -P –policy → explica al kernel qué hacer con los paquetes que no coincidan con ninguna regla.
    -E –rename-chain → cambia el orden de una cadena.
~~~~
Condiciones principales para Iptables:
~~~~
    -p –protocol → la regla se aplica a un protocolo.
    -s –src –source → la regla se aplica a una IP de origen.
    -d –dst –destination → la regla se aplica a una Ip de destino.
    -i –in-interface → la regla de aplica a una interfaz de origen, como eth0.
    -o –out-interface → la regla se aplica a una interfaz de destino.
~~~~
Condiciones TCP/UDP
~~~~
    -sport –source-port → selecciona o excluye puertos de un determinado puerto de origen.
    -dport –destination-port → selecciona o excluye puertos de un determinado puerto de destino.
~~~~
Existen muchas mas condiciones para una configuración avanzada del firewall, pero las elementales ya las tenemos listadas.
Configurar reglas por defecto

La configuración por defecto de un firewall debería ser, traducido al español, “bloquear todo excepto [reglas]”. Para configurar el firewall para que bloquee todas las conexiones debemos teclear:
~~~~
    iptables -P INPUT DROP
    iptables -P FORWARD DROP
    iptables -P OUTPUT DROP
~~~~
Con esto nos quedaremos sin internet, por lo que a continuación debemos empezar a crear reglas permisivas.
Para aplicar una regla que filtre un determinado puerto, debemos ejecutar:
~~~~
    iptables -A INPUT -p tcp –sport 22 22 → crea una regla para el puerto de origen tcp 2222
~~~~
Para bloquear el tráfico procedente de una determinada IP, debemos ejecutar:
~~~~
    iptables -A INPUT -p tcp -m iprange –src-range 192.168.1.13-192.168.2.19 (ejemplo de IP)
~~~~
También podriamos bloquear por MAC con la condición –mac-source.
~~~~
    iptables -A INPUT -m mac –mac-source 00:00:00:00:00:01
~~~~
Una vez ejecutadas las reglas que queramos aplicar, debemos guardarlas tecleando sudo service iptables save
Ver el estado del firewall
~~~~
    iptables -L -n -v
~~~~
El parámetro L muestra las líneas abiertas. V permite recibir más información sobre las conexiones y N nos devuelve las direcciones IP y sus correspondientes puertos sin pasar por un servidor DNS.
Eliminar las reglas existentes

Para borrar toda la configuración del firewall para volver a configurarlo de nuevo debemos teclear:
~~~~
    iptables -F
~~~~
Permitir conexiones entrantes

Teclearemos los siguientes parámetros:
~~~~
    iptables -A INPUT -i [interface] -p [protocolo] –dport [puerto] -m state –state NEW,ESTABLISHED -j ACCEPT

-i: debemos configurar la interfaz, por ejemplo, eth0. Esto es útil en caso de tener varias tarjetas de red, si tenemos sólo una, no tenemos por qué especificar este parámetro.

-p: protocolo. Debemos especificar si el protocolo será TCP o UDP.

–dport: el puerto que queremos permitir, por ejemplo, en caso de HTTP sería el 80.
~~~~
Un ejemplo para permitir las conexiones entrantes desde páginas web:
~~~~
    iptables -A INPUT -i eth0 -p tcp –dport 80 -m state –state NEW,ESTABLISHED -j ACCEPT
~~~~
Permitir las conexiones salientes
~~~~
    iptables -A OUTPUT -o [interfaz] -p [protocolo] –sport [puerto] -m state –state ESTABLISHED -j ACCEPT

-o: debemos configurar la interfaz, por ejemplo, eth0, al igual que en el caso anterior.

-p: protocolo. Debemos especificar si el protocolo será TCP o UDP.

–sport: el puerto que queremos permitir, por ejemplo, en caso de HTTPS sería el 443.
~~~~
Un ejemplo para permitir el tráfico saliente hacia páginas web:
~~~~
    iptables -A OUTPUT -o eth0 -p tcp –sport 80 -m state –state ESTABLISHED -j ACCEPT
~~~~
Permitir los paquetes ICMP

Por defecto, el ping está deshabilitado. Debemos habilitarlo manualmente añadiendo las correspondientes entradas en iptables. Para ello teclearemos:

Para poder hacer ping a otros servidores:
~~~~
    iptables -A OUTPUT -p icmp –icmp-type echo-request -j ACCEPT
~~~~
Para permitir recibir solicitudes de ping de otros equipos:
~~~~
    iptables -A INPUT -p icmp –icmp-type echo-reply -j ACCEPT
~~~~
Permitir que el tráfico interno salga a internet

En el caso de tener 2 tarjetas de red (eth0 en local y eth1 conectada a internet) podemos configurar el firewall para que reenvíe el tráfico de la red local a través de internet. Para ello escribiremos:
~~~~
    iptables -A FORWARD -i eth0 -o eth1 -j ACCEPT
~~~~
Bloquear y prevenir ataques DDoS
~~~~
    iptables -A INPUT -p tcp –dport 80 -m limit –limit 25/minute –limit-burst 100 -j ACCEPT
~~~~
Consultar los paquetes rechazados por iptables

Para saber los paquetes que iptables ha rechazado debemos teclear:
~~~~
    iptables -N LOGGING
~~~~
Ejemplos prácticos:

Cómo bloquear las conexiones entrantes por el puerto 1234:
~~~~
    iptables -A INPUT -p tcp –dport 1234 -j DROP
    iptables -A INPUT -i eth1 -p tcp –dport 80 -j DROP → bloquea en la interfaz eth1
~~~~
Bloquear una dirección IP:
~~~~
    iptables -A INPUT -s 192.168.0.0/24 -j DROP
~~~~
Bloquear una dirección IP de salida:
~~~~
    iptables -A OUTPUT -d 75.126.153.206 -j DROP
~~~~
También podemos bloquear una url, por ejemplo, facebook:
~~~~
    iptables -A OUTPUT -p tcp -d www.facebook.com -j DROP
~~~~
Bloquear el tráfico desde una direccion MAC:
~~~~
    iptables -A INPUT -m mac –mac-source 00:0F:EA:91:04:08 -j DROP
~~~~
Bloquear peticiones ping:
~~~~
    iptables -A INPUT -p icmp –icmp-type echo-request -j DROP
~~~~
UFW (Uncomplicated Firewall) es una herramienta de configuración de firewall para Ubuntu desde la consola, desarrollado para facilitar la configuración del firewall Iptables. Ufw proporciona una manera fácil de crear un firewall basado en host IPv4 o IPv6.

Lo primero que debemos hacer es instalar ufw desde apt-get con:
~~~~
    sudo apt-get install ufw
~~~~
Ahora ejecutamos el firewall tecleando sudo ufw enable. Para parar el firewall, teclearemos sudo ufw disable, y para reiniciarlo, primero lo pararemos y a continuación lo volveremos a arrancar con los comandos especificados.

Una vez tengamos el firewall funcionando, ya podemos comenzar a establecer reglas en su funcionamiento. Para aplicar una regla que permita por defecto todo el tráfico, tecleamos:
~~~~
    sudo ufw default allow
~~~~
Por el contrario, para bloquear todo el tráfico, teclearemos:
~~~~
    sudo ufw default deny
~~~~
Para aplicar reglas a determinados puertos, lo haremos mediante el comando:
~~~~
    sudo ufw allow\deny [puerto]/[protocolo]
~~~~
Ejemplo:
~~~~
    sudo ufw allow 1234/tcp (permite las conexiones del puerto 1234 en tcp)
    sudo ufw deny 4321/udp (bloquea las conexiones del puerto 4321 en udp)
~~~~
Existe un archivo que contiene más reglas predefinidas en la ruta /etc/ufw/before.rules donde, por ejemplo, podemos permitir o bloquear el ping externo. Para ello, pondremos una # delante de la linea -A ufw-before-input -p icmp –icmp-type echo-request -j ACCEPT

Podemos consultar las reglas del firewall desde un terminal tecleando sudo ufw status

Como podemos ver, con UFW es bastante sencillo gestionar a nivel ipv4 e ipv6 nuestro firewall iptables. Todo ello podemos gestionarlo desde un terminal sin necesidad de disponer de una interfaz gráfica, pero aún podemos facilitarlo más con otra aplicación, llamada gufw, que es una interfaz gráfica para ufw que simplifica aún más su uso.

Para instalar gufw debemos escribir en un terminal sudo apt-get install gufw

Una vez instalado, lo ejecutamos escribiendo gufw o buscándolo en el panel de aplicaciones.

La primera ventana que nos muestra el programa nos permite activar y desactivar el firewall, establecer reglas por defecto para el tráfico entrante y saliente (permitir, rechazar y denegar), y, en la parte inferior, crear reglas.

Si pulsamos en el botón + de la parte inferior, accedemos al menú de configuración de reglas. Aqui podemos añadir reglas personalizadas en cuanto a puertos, aplicaciones, ips de origen, etc.

Para añadir una regla por puerto igual que hemos especificado desde terminal, seleccionamos si queremos permitir (allow) o bloquear (deny), si queremos que se filtre el tráfico entrante o saliente, el protocolo, ya sea tcp o udp y el puerto a filtrar.

Las posibilidades de iptables son prácticamente infinitas, y la dificultad de configuración aumenta exponencialmente según sean de complejas las configuraciones que queremos realizar. Desde RedesZone esperamos que con este pequeño tutorial podáis configurar vuestro firewall iptables a nivel básico de forma sencilla.
