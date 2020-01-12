![](images/Practica2.png)

Definimos la topología:

**Máquina anfitriona:**

Creamos el archivo de configuración "net.conf" con el siguiente contenido:
<pre><code>defsw br12 uml1.0 uml2.2
defsw br15 uml1.1 uml5.1
defsw br23 uml2.1 uml3.0
defsw br24 uml2.0 uml4.1
defsw br34 uml3.1 uml4.0
defsw br35 uml3.2 uml5.0</code></pre>

Limpiamos configuraciones viejas con el comando:
<pre><code>sudo ifovsdel</code></pre>

Comprobamos que la sintaxis sea correcta con:
<pre><code>sudo ifovsparse net.conf</code></pre>

Creamos y lanzamos los directorios de las máquinas con:
<pre><code>mkdir uml{1..5}
lanza {1..5}</code></pre>

## **Configuración BGP**

Lo primero es activar el demonio zebra de bgpd en todas las máquinas. Esto es, colocando el flag a 'yes' bgpd, editando el archivo /etc/quagga/daemons. Despues hacemos restart del demonio zebra con:
<pre><code>systemctl restart quagga</code></pre>

Después hay que levantar los interfaces y asignar direcciones a las máquinas.
