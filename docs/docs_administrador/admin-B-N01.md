# Especificaciones de la máquina B-N01:

- Ubuntu server  
- 8 CPUs  
- 12GB RAM  
- Disk Bus: Virtio

Tarjetas de red:  
Default  
Wireguard VPN  
ASIXc2-ITB1

# Objetivos:

Crear una base de datos (MySQL)  
Hacer el DHCP con docker (contenedor 1\)  
Hacer el DNS con docker (contenedor 2\)

# Paquetes instalados:

Instalar diferentes paquetes como:

* nano  					sudo apt install nano  
* ping   					sudo apt install iputils-ping  
* bash-completion sudo 		sudo apt install bash-completion  
* MySQL				sudo apt install mysql-server

## Crear usuario bchecker:

sudo adduser bchecker  
Password: bchecker121  
<img width="414" height="263" alt="1 Crea_User_bchecker" src="https://github.com/user-attachments/assets/dbad8274-f4e2-4ef8-930c-f445bdd0790e" />


sudo usermod \-aG sudo bchecker  
<img width="492" height="138" alt="2 Permisos_sudo_bchecker" src="https://github.com/user-attachments/assets/15357356-a81b-45b8-9279-79b47059beb4" />


## Cambiar hostname a B-N01

sudo hostnamectl hostname B-N01  
sudo nano /etc/hosts y cambiamos ubuntu por B-N01  
<img width="338" height="44" alt="3 1 Cambio_hostname" src="https://github.com/user-attachments/assets/6b0f403a-bce4-4552-ae85-ec03cd504ff2" />

<img width="496" height="52" alt="3 2 Cambio_hostname" src="https://github.com/user-attachments/assets/84e7a4f4-5876-48dd-807f-4583f7b9fd72" />


## Instalamos MySQL:

Con sudo apt install mysql-server instalamos MySQL:

<img width="540" height="409" alt="4 Instalamos_MySQL" src="https://github.com/user-attachments/assets/3c5b6ae2-8dc5-4da5-ad92-56e66d0b5e5a" />


## Configuración del netplan:

sudo nano /etc/netplan/00-installer-config.yaml  
<img width="670" height="274" alt="5 1 Fichero_Netplan" src="https://github.com/user-attachments/assets/a7007e12-2f93-4e56-8904-a82914c36313" />

Las partes comentadas con \# son para cuando esté listo el router, la configuración comentada son el gateway y el servidor DNS.

sudo netplan apply

<img width="774" height="393" alt="5 2 Tabla_de_IPs" src="https://github.com/user-attachments/assets/b4759081-8e89-4282-b385-9005d6292d39" />

Hacemos ping 8.8.8.8 (google) para comprobar que funcione correctamente  
<img width="473" height="151" alt="5 3 Ping" src="https://github.com/user-attachments/assets/04c5b5e9-6a0f-4164-ae64-b8c76968e008" />


## Instalamos SSH para crear las claves públicas y privadas

Instalamos SSH con sudo apt install ssh  
<img width="960" height="724" alt="6 1 Instalamos SSH" src="https://github.com/user-attachments/assets/d3df3afd-d291-4140-9dae-6d0ba8a54faf" />


Comprobamos que el servicio esté activo antes de empezar con las claves  
<img width="961" height="199" alt="6 2 SSH_activo" src="https://github.com/user-attachments/assets/60a40727-123b-461e-b7f0-3a258fb13aa3" />


Creamos la clave pública y privada con “ssh-keygen” (se guardarán en /home/bchecker/.ssh/id\_rsa):  
<img width="961" height="348" alt="6 3 ssh-Keygen" src="https://github.com/user-attachments/assets/4f94f480-093b-469a-87e7-d5cc41ae2ef0" />


## Cuentas MySQL:

<img width="1478" height="177" alt="Captura de pantalla de 2026-11-09 15-58-23" src="https://github.com/user-attachments/assets/6c0eefce-b22c-4242-80b4-dce8c7cf1019" />

[Excel](https://docs.google.com/spreadsheets/d/1pSMSdBQ_dn-vCXINd7joOUrrGLxHVaYlgL7RDUfkY08/edit?usp=sharing)

## admin\_db:



| CREATE USER | Crea el usuario |
| :---- | :---- |
| ‘admin\_db’ | Nombre del usuario |
| ‘localhost’ | Desde dónde se podrá acceder al usuario |
| IDENTIFIED BY | Que contraseña hará falta para acceder |
| ‘admin1234’ | Contraseña para acceder |

Permisos de admin\_db:  


| GRANT | Da los siguientes permisos |
| :---- | :---- |
| ALL PRIVILEGES | Otorga todos los privilegios posibles |
| ON \*.\* | Define las base de datos y las tablas que se podrán manipular (\* significa todas las posibles) |
| TO | A quien van dirigidos los privilegios |
| ‘admin\_db’@’localhost’ | Usuario que recibirá los privilegios |

## app\_user



| CREATE USER | Crea el usuario |
| :---- | :---- |
| ‘app\_user’ | Nombre del usuario |
| ‘192.168.2.2’ | Desde dónde se podrá acceder al usuario |
| IDENTIFIED BY | Que contraseña hará falta para acceder |
| ‘app1234’ | Contraseña para acceder |



| GRANT | Da los siguientes permisos |
| :---- | :---- |
| SELECT | Otorga permisos de lectura |
| INSERT | Otorga permisos para insertar |
| UPDATE | Otorga permisos para modificar |
| DELETE | Otorga permisos para eliminar datos |
| ON barcelona.\* | Estos permisos sólo los tendrá dentro de la base de datos **“barcelona\_educacio”** |
| TO ‘app\_user’@’192.168.2.2’ | Solo si el usuario se conecta desde 192.168.2.2 con el propio usuario |

## Lectura


| CREATE USER | Crea el usuario |
| :---- | :---- |
| ‘lectura’ | Nombre del usuario |
| ‘192.168.1.101’ | Desde dónde se podrá acceder al usuario |
| IDENTIFIED BY | Que contraseña hará falta para acceder |
| ‘lectura1234’ | Contraseña para acceder |

![][image18]

| GRANT | Da los siguientes permisos |
| :---- | :---- |
| SELECT | Otorga permisos de lectura |
| ON barcelona.\* | Estos permisos sólo los tendrá dentro de la base de datos barcelona\_educacio |
| TO ‘app\_user’@’192.168.1.101’ | Solo si el usuario se conecta desde 192.168.1.101 con el propio usuario |

## Backup\_user

| CREATE USER | Crea el usuario |
| :---- | :---- |
| ‘backup\_user’ | Nombre del usuario |
| ‘localhost’ | Desde dónde se podrá acceder al usuario |
| IDENTIFIED BY | Que contraseña hará falta para acceder |
| ‘backup1234’ | Contraseña para acceder |

![][image19]

| GRANT | Da los siguientes permisos. |
| :---- | :---- |
| SELECT | Permite consultar y leer los datos de las tablas. |
| LOCK TABLES | Permite bloquear tablas durante determinadas operaciones, evitando modificaciones concurrentes mientras se realiza el bloqueo. |
| SHOW VIEW | Permite consultar la definición de las vistas mediante SHOW CREATE VIEW. |
| EVENT | Permite crear, modificar y eliminar eventos programados de MySQL. |
| TRIGGER | Permite crear y eliminar triggers (disparadores) asociados a las tablas. |
| ON barcelona.\* | Estos permisos sólo los tendrá dentro de la base de datos **“barcelona\_educacio”.** |
| TO ‘backup\_user’@’localhost’ | Solo si el usuario se conecta desde localhost (máquina local) con el propio usuario. |

### Hacer backup

mysqldump \-u backup\_user \-p \--single-transaction \--no-tablespaces barcelona\_educacio \> /ruta/cualquiera/Nombre\_del\_backup.sql

### Restaurar backup

mysql \-u admin\_db \-p barcelona\_educacio \< /ruta/cualquiera/Nombre\_del\_backup.sql

### Archivo mysqld.conf:

<img width="957" height="729" alt="7 3 2 Fichero_mysqld cnf" src="https://github.com/user-attachments/assets/0dffc3fe-3ed0-4f40-ae92-0d4b9e90292f" />
 
Editamos el bind address para que mysql conecte por la IP  192.168.1.2  
![][image21]

Reiniciamos el servicio y comprobamos que se hayan aplicado los cambios  
<img width="706" height="81" alt="7 3 3 Prueba_funciona_3306" src="https://github.com/user-attachments/assets/f1b41ee9-7370-4f4a-9852-6d118d4bb944" />


# Importar .CSV

## Wget .CSV

wget \-O educacion.csv "https\://opendata-ajuntament.barcelona.cat/data/dataset/f36b60f2-9541-4d08-b0f9-b0a9313fab3d/resource/29d9ff10-6892-4f16-9012-d5c4997857e7/download"

## Importar .CSV

Antes de importar a MySQL hace falta cambiar la versión UTF ( de UTF-16 que era el original a UTF-8 para que MySQL pueda importarlo correctamente)  
<img width="608" height="52" alt="8 1 De_UTF-16_a_UTF-8" src="https://github.com/user-attachments/assets/33d06480-7e63-4f07-b937-4e1b495439ae" />

   
Habilitamos la opción local\_infile=1 para que la base de datos pueda importar archivos que estén dentro de la máquina

Con ayuda de la IA he hecho un script que cree desde 0 la base de datos para poder importar el csv.  
<img width="675" height="189" alt="image" src="https://github.com/user-attachments/assets/3edc5754-8eda-4e8d-981c-6813efd6a98b" />


Importamos la base de datos  
<img width="501" height="154" alt="image" src="https://github.com/user-attachments/assets/7960468f-cebb-4a6c-9749-ca0263355fd9" />


Salida del comando cat  
<img width="531" height="706" alt="image" src="https://github.com/user-attachments/assets/037e3cf8-0b66-4112-b5c2-4aabc59fc6aa" />


Todos los archivos se han cargado correctamente (2709)  
<img width="267" height="266" alt="image" src="https://github.com/user-attachments/assets/52f9b368-297a-47ef-9cc8-e312f1490e3e" />

