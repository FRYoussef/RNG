![](images/Practica1_1.png)
![](images/Practica1_2.png)

Definimos la topología:

**Máquina anfitriona:**

Creamos el archivo de configuración "net.conf" con el siguiente contenido:
<pre><code>defsw br12 uml1.0 uml2.2
defsw br23 uml2.1 uml3.0
defsw br24 uml2.0 uml4.1
defsw br34 uml3.1 uml4.0</code></pre>

Limpiamos configuraciones viejas con el comando:
<pre><code>sudo ifovsdel</code></pre>

Comprobamos que la sintaxis sea correcta con:
<pre><code>sudo ifovsparse net.conf</code></pre>

Creamos y lanzamos los directorios de las máquinas con:
<pre><code>mkdir uml{1..4}
lanza {1..4}</code></pre>

## **Configuración BGP**

Lo primero es activar el demonio zebra de bgpd en todas las máquinas. Esto es, colocando el flag a 'yes' bgpd, editando el archivo /etc/quagga/daemons. Despues hacemos restart del demonio zebra con:
<pre><code>systemctl restart quagga</code></pre>

Después hay que levantar los interfaces y asignar direcciones a las máquinas.

**UML1**

<pre><code>vtysh
# configure terminal
# interface eth0
# ip address 10.0.0.1/24
# exit
# router bgp 65512
# neighbor 10.0.0.2 remote-as 65513
# network 10.12.0.0/16
# network 172.16.12.0/24
# network 192.168.0.0/23
# address-family ipv6
# neighbor 10.0.0.2 activate
# network 2001:db8:12::/52
# network 2001:db8:12:1000::/52
# network 2001:db8:12:2000::/52
# end
# write</code></pre>
