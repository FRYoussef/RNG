![](images/Examen1.png)

Definimos la topología:

**Máquina anfitriona:**

Creamos el archivo de configuración "net.conf" con el siguiente contenido:
<pre><code>defsw br12 uml1.0 uml2.3
defsw br13 uml1.1 uml3.3
defsw br1_0 uml1.2
defsw br2_0 uml2.0
defsw br24 uml2.1 uml4.2
defsw br23 uml2.2 uml3.0
defsw br3_0 uml3.1
defsw br35 uml3.2 uml5.2
defsw br4_0 uml4.0
defsw br45 uml4.1 uml5.0
defsw br5_0 uml5.1/code></pre>

Limpiamos configuraciones viejas con el comando:
<pre><code>sudo ifovsdel</code></pre>

Comprobamos que la sintaxis sea correcta con:
<pre><code>sudo ifovsparse net.conf</code></pre>

Creamos y lanzamos los directorios de las máquinas con:
<pre><code>mkdir uml{1..5}
lanza {1..5}</code></pre>

Lo primero es activar los demonios zebra de ospf(uml{2..5}), ospf6(uml{4..5}) y bgp(todas). Esto es, colocando los flags a 'yes', editando el archivo /etc/quagga/daemons. Despues hacemos reiniciamos el demonio zebra con:
<pre><code>systemctl restart quagga</code></pre>

## Configuración del encaminamiento interior (OSPFv2 y OSPFv3)
