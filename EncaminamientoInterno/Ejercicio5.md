![](EncaminamientoInterno/images/ej5_1-OSPF.png)
![](EncaminamientoInterno/images/ej5-OSPF.png)

Definimos la topología:

**Máquina anfitriona:**

Creamos el archivo de configuración "net.conf" con el siguiente contenido:
<pre><code>
defsw br15 uml1.0 uml5.3
defsw br14 uml1.1 uml4.0
defsw br1_0 uml1.2
defsw br1_1 uml1.3
defsw br2_0 uml2.0
defsw br2_1 uml2.1
defsw br23 uml2.2 uml3.0
defsw br25 uml2.3 uml5.1
defsw br3_0 uml3.1
defsw br3_1 uml3.2
defsw br4_0 uml4.1
defsw br4_1 uml4.2
defsw br5_0 uml5.0
defsw br5_1 uml5.2
</code></pre>

Limpiamos configuraciones viejas con el comando:
<pre><code>sudo ifovsdel</code></pre>

Comprobamos que la sintaxis sea correcta con:
<pre><code>sudo ifovsparse net.conf</code></pre>

Creamos y lanzamos los directorios de las máquinas con:
<pre><code>mkdir uml{1..5}
lanza {1..5}</code></pre>

**UML{1..5}:**

Lo primero es activar los demonios zebra de ospf y ospf6. Esto es, colocando los flags a 'yes' de ospfd y ospf6d, editando el archivo /etc/quagga/daemons. Despues hacemos reiniciamos el demonio zebra con:
<pre><code>systemctl restart quagga</code></pre>

**UML1:**

Configuramos el área 1 como totally-stub para ipv4.

<pre><code>vtysh
# configure terminal
# interface eth0
# ip address 10.0.0.1/24
# exit
# interface eth1
# ip address 10.0.1.1/24
# exit
# interface eth2
# ip address 192.168.0.1/24
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8::/64
# ipv6 address 2001:db8::ff:fe00:1/64
# ipv6 ospf6 passive
# exit
# interface eth3
# ip address 192.168.1.1/24
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:1::/64
# ipv6 address 2001:db8:1::ff:fe00:1/64
# ipv6 ospf6 passive
# exit
# ip forwarding
# ipv6 forwarding
# router ospf
# router-id 0.0.0.1
# network 10.0.0.0/24 area 0
# network 10.0.1.0/24 area 1
# network 192.168.1.0/24 area 0
# network 192.168.0.0/24 area 0
# passive-interface eth2
# passive-interface eth3
# area 1 stub no-summary
# exit
# router ospf6
# router-id 0.0.0.1
# interface eth0 area 0.0.0.0
# interface eth1 area 0.0.0.1
# interface eth2 area 0.0.0.0
# interface eth3 area 0.0.0.0
# end
# write
</code></pre>

**UML2:**

También vamos a definir el área 2 como Stub para ipv4.

<pre><code>vtysh
# configure terminal
# interface eth0
# ip address 192.168.3.2/24
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:3::/64
# ipv6 address 2001:db8:3::ff:fe00:3/64
# ipv6 ospf6 passive
# exit
# interface eth1
# ip address 192.168.2.2/24
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:2::/64
# ipv6 address 2001:db8:2::ff:fe00:2/64
# ipv6 ospf6 passive
# exit
# interface eth2
# ip address 10.0.2.2/24
# exit
# interface eth3
# ip address 10.0.3.2/24
# exit
# ip forwarding
# ipv6 forwarding
# router ospf
# router-id 0.0.0.1
# network 10.0.2.0/24 area 2
# network 10.0.3.0/24 area 0
# network 192.168.2.0/24 area 0
# network 192.168.3.0/24 area 0
# passive-interface eth0
# passive-interface eth1
# area 2 stub
# exit
# router ospf6
# router-id 0.0.0.2
# interface eth0 area 0.0.0.0
# interface eth1 area 0.0.0.0
# interface eth2 area 0.0.0.2
# interface eth3 area 0.0.0.0
# end
# write
</code></pre>

**UML3:**

Definimos el área 2 como Stub para ipv4.

<pre><code>vtysh
# configure terminal
# interface eth0
# ip address 10.0.2.3/24
# exit
# interface eth1
# ip address 192.168.5.3/24
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:5::/64
# ipv6 address 2001:db8:5::ff:fe00:3/64
# ipv6 ospf6 passive
# exit
# interface eth2
# ip address 192.168.4.3/24
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:4::/64
# ipv6 address 2001:db8:4::ff:fe00:3/64
# ipv6 ospf6 passive
# exit
# ip forwarding
# ipv6 forwarding
# router ospf
# router-id 0.0.0.1
# network 10.0.2.0/24 area 2
# network 192.168.4.0/24 area 2
# network 192.168.5.0/24 area 2
# passive-interface eth2
# passive-interface eth1
# area 2 stub
# exit
# router ospf6
# router-id 0.0.0.3
# interface eth0 area 0.0.0.2
# interface eth1 area 0.0.0.2
# interface eth2 area 0.0.0.2
# end
# write
</code></pre>

**UML4:**

Vamos a configurar el router como totally-stub en ipv4.

<pre><code>vtysh
# configure terminal
# interface eth0
# ip address 10.0.1.4/24
# exit
# interface eth1
# ip address 192.168.7.4/24
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:7::/64
# ipv6 address 2001:db8:7::ff:fe00:4/64
# ipv6 ospf6 passive
# exit
# interface eth2
# ip address 192.168.6.4/24
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:6::/64
# ipv6 address 2001:db8:6::ff:fe00:4/64
# ipv6 ospf6 passive
# exit
# ip forwarding
# ipv6 forwarding
# router ospf
# router-id 0.0.0.4
# network 10.0.1.0/24 area 1
# network 192.168.6.0/24 area 1
# network 192.168.7.0/24 area 1
# passive-interface eth2
# passive-interface eth1
# area 1 stub
# exit
# router ospf6
# router-id 0.0.0.4
# interface eth0 area 0.0.0.1
# interface eth1 area 0.0.0.1
# interface eth2 area 0.0.0.1
# end
# write
</code></pre>

**UML5:**

<pre><code>vtysh
# configure terminal
# interface eth0
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:a::/64
# ipv6 address 2001:db8:a::ff:fe00:5/64
# ipv6 ospf6 passive
# exit
# interface eth1
# ip address 10.0.3.5/24
# exit
# interface eth2
# no ipv6 nd suppress-ra
# ipv6 nd prefix 2001:db8:8::/64
# ipv6 address 2001:db8:8::ffff:fe00:5/64
# ipv6 ospf6 passive
# exit
# interface eth3
# ip address 10.0.0.5/24
# exit
# ip forwarding
# ipv6 forwarding
# router ospf
# router-id 0.0.0.5
# network 10.0.0.0/24 area 0
# network 10.0.3.0/24 area 0
# passive-interface eth0
# passive-interface eth2
# exit
# router ospf6
# router-id 0.0.0.5
# interface eth1 area 0.0.0.0
# interface eth3 area 0.0.0.0
# redistribute connected
# end
# write
</code></pre>


**Comprobación**

Podemos comprobar la vecindad con los comandos:
<pre><code>
# show ip ospf neighbor
# show ipv6 ospf6 neighbor
</code></pre>

Y con los siguientes comandos podemos comprobar la tabla de rutas, para ver que todos los prefijos son visibles:
<pre><code>
# show ip ospf route
# show ipv6 ospf6 route
</code></pre>

