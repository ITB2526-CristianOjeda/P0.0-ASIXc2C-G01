
## Tarea 2.1: Despliegue del Servidor Web (W-N01)

Instalar y configurar el Servidor Web W-N06 en la red DMZ.

Desplegamos el servidor web con la interfaz correspondiente a la DMZ (ITB1a)

![img_037.png](../img/img_037.png)

Para el servicio web usaremos NGINX, para instalarlo hay que usar los siguientes comandos

| sudo systemctl enable nginx<br>sudo systemctl start nginx<br>sudo systemctl status nginx |
| --- |

![img_038.png](../img/img_038.png)

Comprobación

Desde el cliente linux CLI-LNX-01

![img_039.png](../img/img_039.png)

### Tarea 2.1.1: Despliegue SSH al Servidor Web (W-N01)

Asegurar que el acceso SSH esté habilitado para administración en el servidor web W-N01.

Instalamos el servicio openssh server y habilitamos el servicio

| sudo apt install openssh-server -y |
| --- |

![img_040.png](../img/img_040.png)

| sudo systemctl enable ssh<br>sudo systemctl start ssh |
| --- |

![img_041.png](../img/img_041.png)

Comprobación

![img_042.png](../img/img_042.png)

![img_043.png](../img/img_043.png)

### Tarea 2.5.2: Configuración de Credenciales del Usuario bchecker W-N01

Creamos el usuario bchecker en el W-N01

| sudo adduser bchecker<br>Password: bchecker123 |
| --- |

![img_055.png](../img/img_055.png)

Agregamos el usuario bchecker al grupo sudo

| sudo usermod -aG sudo bchecker |
| --- |

![img_056.png](../img/img_056.png)
