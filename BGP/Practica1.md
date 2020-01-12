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


**UML2**

<pre><code>vtysh
# configure terminal
# interface eth0
# ip address 10.0.2.2/24
# exit
# interface eth1
# ip address 10.0.1.2/24
# exit
# interface eth2
# ip address 10.0.0.2/24
# exit
# router bgp 65513
# neighbor 10.0.0.1 remote-as 65512
# neighbor 10.0.1.3 remote-as 65514
# neighbor 10.0.2.4 remote-as 65515
# network 10.13.0.0/16
# address-family ipv6
# neighbor 10.0.0.1 activate
# neighbor 10.0.1.3 activate
# neighbor 10.0.2.4 activate
# network 2001:db8:13:1::/64
# network 2001:db8:13:2::/64
# end
# write</code></pre>


**UML3**

<pre><code>vtysh
# configure terminal
# interface eth0
# ip address 10.0.1.3/24
# exit
# interface eth1
# ip address 10.0.45.3/24
# exit
# router bgp 65514
# neighbor 10.0.1.2 remote-as 65513
# neighbor 10.0.45.4 remote-as 65515
# network 10.14.0.0/16
# network 172.16.14.0/24
# address-family ipv6
# neighbor 10.0.1.2 activate
# neighbor 10.0.45.4 activate
# network 2001:db8:14::/64
# network 2001:db8:14:2::/64
# network 2001:db8:14:3::/64
# end
# write</code></pre>


**UML4**

<pre><code>vtysh
# configure terminal
# interface eth0
# ip address 10.0.45.4/24
# exit
# interface eth1
# ip address 10.0.2.4/24
# exit
# router bgp 65515
# neighbor 10.0.2.2 remote-as 65513
# neighbor 10.0.45.3 remote-as 65514
# network 10.15.0.0/16
# network 172.16.15.0/24
# network 192.168.128.0/23
# address-family ipv6
# neighbor 10.0.2.2 activate
# neighbor 10.0.45.3 activate
# network 2001:db8:15::/64
# network 2001:db8:15:1::/64
# network 2001:db8:15:2::/64
# end
# write</code></pre>

Podemos comprobar que las rutas son alcanzables con:
<pre><code># show ip bgp
# show ipv6 bgp</code></pre>
