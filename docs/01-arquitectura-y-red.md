# 01_Arquitectura y red

Para organizar la arquitectura de red de este proyecto, hemos tenido en cuenta cuántas redes son necesarias para ubicar todos y cada uno de los servidores, basándonos en la coherencia y en la seguridad que aporta dividirlos en las siguientes redes:
<img width="1003" height="646" alt="image" src="https://github.com/user-attachments/assets/07cfe625-3c6c-4266-acce-c1ad7ffd388c" />

## Red DMZ ##
Servidor web (W-N01)
Servidor FTP (F-N01)

Hemos juntado en la misma red estos dos servidores debido a la sinergia que hay entre ellos a la hora de trabajar: el servidor FTP (File Transfer Protocol) le proporciona todo el material posible al servidor web para que este pueda ser actualizado de manera natural y pueda publicar los datos que F-N01 vaya actualizando. Otra ventaja de tener estos dos servidores en la misma red es el rendimiento de la transmisión de datos entre ellos: al hacerlo así, los archivos compartidos tardan menos en ser enviados y recibidos por la otra máquina, lo que ofrece una mayor eficiencia y productividad. Por último, al dividir estos dos servidores en vez de hacerlo todo en una misma máquina, reforzamos la seguridad de ambos por igual, evitando así dar acceso directo a un posible atacante desde cualquiera de las dos máquinas.

### En resumen: ###
· Ambos servidores tienen sinergia entre si
· Juntos rinden mejor a la hora de transmitir y publicar archivos
· Reforzamos la seguridad del sistema
