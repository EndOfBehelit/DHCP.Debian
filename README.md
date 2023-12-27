# DHCP.Debian
<h1>Configuracion de isc-dhcp-server en Debian</h1>

<p>Para empezar con la instalacion debemos descargar el paquete.<br><br>
            <pre></pre><strong>sudo apt install isc-dhcp-server</strong></pre></p>
<br>
<p>Ahora deberemos hacer ciertas modificaciones en la configuración red, modificando el archivo <strong>/etc/network/interfaces</strong><br>
Debemos configurar nuestro Debian como router asignándole la IP .1 de manera estática.</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/b8a57b13-687f-47ca-b004-6a831a6d047b" alt="Configuración estática de interfaces" width="700" height="500px"/>

