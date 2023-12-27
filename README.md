# DHCP.Debian
<h1>Configuracion de isc-dhcp-server en Debian</h1>

<h2>Instalar isc-dhcp-server<br></h2>
            <pre><strong>sudo apt install isc-dhcp-server</strong></pre>
<br>
            <p>Puedes ver el estado del servicio o reiniciarlo en cualquier momento con estos comandos</p>
            <pre><strong>sudo systemctl status isc-dhcp-server</strong></pre>
            <pre><strong>sudo systemctl restart isc-dhcp-server</strong></pre>
<br>
<p>Ahora deberemos hacer ciertas modificaciones en la configuración red, modificando el archivo <strong>/etc/network/interfaces</strong><br>
Debemos configurar nuestro Debian como router asignándole la IP .1 de manera estática.</p> <br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/b8a57b13-687f-47ca-b004-6a831a6d047b" alt="Configuración estática de interfaces" width="700" height="500px"/> <br><br>

<h2>Configuración de interfaz en <strong>/etc/default/isc-dhcp-server</strong></h2>
<p>En este caso usamos la IPv4 y la interfaz modificada en el paso anterior como router es la "ens192"</p>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/a6a8424b-ab42-4e08-8138-50422951cc64" alt="Configuración de interfaz a usar en servicio DHCP" width="700" height="500px"/> <br><br>

<h2>Configuración de <strong>/etc/dhcp/dhcpd.conf</strong></h2>
<p>Debemos configurar los en este archivo la subnet en la que usar el DHCP, los host o reservas y las características del servicio como el lease time</p><br>
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
</pre><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/e8b62f2d-549e-4d00-955d-5dbe12281794" alt="Configuración de interfaz a usar en servicio DHCP" width="700" height="500px"/> <br><br>
<li>Configuración de subred a usar DHCP</li>
<pre>
#SUBNET debian, no todas las opciones son obligatorias, pueden usarse las que se necesiten
#Debe ponerse la red, dirección acabada en .0
            subnet 192.168.1.0 netmask 255.255.255.0 {
            #Rango en dentro de la red dada, en la que se usará e servició DHCP
                        range 192.168.1.1 192.168.1.255;
            #Router que se usa en la red, en este caso el router el también el que da el servicio DHCP
                        option routers 192.168.1.1;
            #Dirección a usar como brodcast
                        option brodcast-address 192.168.1.255;
            #Pueden configuirarse DNS o lease time diferentes a los default de la cabecera
                        option domain-name-servers ip;
                        max-lease-time 222;
            }
</pre>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/9cda93b6-395f-4e52-b28d-6fc9332a16e4" alt="Configuración de interfaz a usar en servicio DHCP" width="700" height="500px"/><br><br>
<li>Creación de hosts o reservas</li>
<pre>
#Reserva de una IP para alguna máquina, pueden configurarse como antes lease time o DNS personalizados para hosts
            host máquina1 {
                        hardware ethernet 00:00:00:00:00:00;
                        fixed-address 192.168.1.10;
            #Opciones no obligatorias
                        option domain-name-servers ip;
                        max-lease-time 22222;
            }
</pre>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/64ef19aa-bd97-456a-9ce2-0b5f7e4f30a1" alt="Configuración de interfaz a usar en servicio DHCP" width="700" height="500px"/> <br><br>

<h2>Configuración de una máquina cliente (Windows y Ubuntu) en el servicio de DNS</h2>
<p>Configuración red de Windows</p>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/fe50dfeb-4dc6-48a4-9f33-91b81d0bed76" alt="Configuración de interfaz a usar en servicio DHCP" width="700" height="500px"/> <br><br>
<p>Configuración red de Ubuntu</p>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/EndOfBehelit/DHCP.Debian/assets/154753826/dc64568b-42c1-4ad4-aee4-12f5af37969e" alt="Configuración de interfaz a usar en servicio DHCP" width="700" height="500px"/> <br><br>

