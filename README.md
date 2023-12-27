# DHCP.Debian
<h1>Configuracion de isc-dhcp-server en Debian</h1>

<h2>Instalar isc-dhcp-server<br><br></h2>
            <pre><strong>sudo apt install isc-dhcp-server</strong></pre>
<br>
<p>Ahora deberemos hacer ciertas modificaciones en la configuración red, modificando el archivo <strong>/etc/network/interfaces</strong><br>
Debemos configurar nuestro Debian como router asignándole la IP .1 de manera estática.</p> <br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/b8a57b13-687f-47ca-b004-6a831a6d047b" alt="Configuración estática de interfaces" width="700" height="500px"/> <br><br>

<h2>Configuración de interfaz en <strong>/etc/default/isc-dhcp-server</strong></h2><br><br>
<p>En este caso usamos la IPv4 y la interfaz modificada en el paso anterior como router es la "ens192"</p>
<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/a6a8424b-ab42-4e08-8138-50422951cc64" alt="Configuración de interfaz a usar en servicio DHCP" width="700" height="500px"/> <br><br>

<h2>Configuración de <strong>/etc/dhcp/dhcpd.conf</strong></h2>
<p>Debemos configurar los en este archivo la subnet en la que usar el DHCP, los host o reservas y las características del servicio como el lease time</p><br><br>
<li>Cabecera del archivo, lease time general, DNSs...</li>
<pre>
            #Esto permite actualizar los registros de DNS de manera automática, no lo necesitamos
            ddns-update-style none;
            #Nombre e IP de los dns a usar, el nombre no es obligatorio, con la ip valdría
            option domain-name "nombre_del_dominio";
            option domain-name-servers ip1 ip2;
            #Lease time en segundos
            default-lease-time 22222;
            max-lease.time 222222;
            #Authoritative, si el servidor es de nuestra red (obviamente sí), debe ser authoritative
            authoritative;
</pre>
