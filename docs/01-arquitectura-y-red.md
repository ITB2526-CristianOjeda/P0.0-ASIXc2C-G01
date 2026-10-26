# 01_Arquitectura y red

Para organizar la arquitectura de red de este proyecto, hemos tenido en cuenta cuántas redes son necesarias para ubicar todos y cada uno de los servidores, basándonos en la coherencia y en la seguridad que aporta dividirlos en las siguientes redes:
<img width="1003" height="646" alt="image" src="https://github.com/user-attachments/assets/07cfe625-3c6c-4266-acce-c1ad7ffd388c" />

## Red DMZ ##
- Servidor Web (W-N01)

- Servidor FTP (F-N01)

Hemos juntado en la misma red estos dos servidores debido a la sinergia que hay entre ellos a la hora de trabajar: el servidor FTP (File Transfer Protocol) le proporciona todo el material posible al servidor web para que este pueda ser actualizado de manera natural y pueda publicar los datos que F-N01 vaya actualizando. Otra ventaja de tener estos dos servidores en la misma red es el rendimiento de la transmisión de datos entre ellos: al hacerlo así, los archivos compartidos tardan menos en ser enviados y recibidos por la otra máquina, lo que ofrece una mayor eficiencia y productividad. Por último, al dividir estos dos servidores en vez de hacerlo todo en una misma máquina, reforzamos la seguridad de ambos por igual, evitando así dar acceso directo a un posible atacante desde cualquiera de las dos máquinas.

### En resumen: ###
·  Ambos servidores tienen sinergia entre si

·  Juntos rinden mejor a la hora de transmitir y publicar archivos

·  Reforzamos la seguridad del sistema


## Intranet ##
- Servidor BBDD (B-N01)

- Servidor DHCP/DNS/Docker (D-N01)

- Cliente Windows (CLI-WIN-N01)

- Cliente Linux (CLI-LNX-N01)

La razón por la que las máquinas B-N01 y D-N01 están en la intranet es que no son servidores que deban dar la cara al público, como sí lo son el servidor web y el FTP. Son servidores que tienen que estar lo más protegidos posible para evitar filtrar información valiosa. Además, al tener los clientes en la misma red que el DHCP, no hará falta configurar un DHCP Relay: con la configuración del propio servidor bastaría. Por último, los servicios internos de estas máquinas seguirán funcionando aunque la conexión externa se caiga.

### En resumen: ###
· No  están expuestas al público por estar en la intranet.

· Al compartir red con los clientes, no hace falta DHCP Relay.

· No necesitan conexión externa para funcionar.

## Router (R-N01) ##

El router R-N01 conecta las tres redes principales: la DMZ, la intranet e Internet. Actúa como gateway de la intranet y de la DMZ, y es el único punto de paso entre las tres, de modo que todo el tráfico entre redes pasa por él y puede ser filtrado y controlado desde un único lugar. Esto resulta especialmente útil contra ataques externos, ya que cualquier intento de acceso desde Internet tiene que atravesar el router, que es donde se puede detectar y filtrar.

### En Resumen ###
· Conecta la DMZ, la intranet e Internet.

· Es el gateway de ambas redes.

· Centraliza el control del tráfico entre zonas.
