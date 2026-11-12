# Manual de administración del servidor B-N01

Servidor de base de datos MySQL · Proyecto P0.0-ASIXc2C-G01 · Tarea 2.3.1

## 1. Objetivo y alcance

Este manual describe cómo administrar el servidor **B-N01**, que aloja la base de datos MySQL del proyecto. Está dirigido a los administradores del sistema y recoge la configuración actual, los procedimientos habituales y la resolución de problemas.

**Cubre:** configuración del servicio, usuarios y permisos, firewall, importación de datos, copias de seguridad y mantenimiento.

**No cubre:** la configuración de W-N01 (web), F-N01 (FTP), D-N01 (DHCP/DNS/Docker) ni del router R-N01, salvo lo que afecta al acceso a la base de datos.


## 2. Ficha del servidor

| Elemento | Valor |
| --- | --- |
| Nombre | B-N01 |
| Función | Servidor de base de datos |
| Red | INTRANET 192.168.1.0/24 |
| Dirección IP | 192.168.1.2/24 |
| Gateway | 192.168.1.1 (R-N01) |
| Sistema operativo | Ubuntu Server |
| Software | MySQL Server 8.0.46 |
| Puerto de servicio | 3306/TCP |
| Puerto X Protocol | 33060/TCP, solo en 127.0.0.1 |
| Usuario del sistema | bchecker |
| Cntraseña | bchecker121 |
| Base de datos | barcelona_educacio |
| Tabla principal | equipamientos |

## 3. Entorno de red y accesos permitidos

| Equipo | Función | IP | Acceso a MySQL |
| --- | --- | --- | --- |
| W-N01 | Servidor web (DMZ) | 192.168.2.2 | Sí, con " app_user " |
| F-N01 | Servidor FTP (DMZ) | 192.168.2.3 | Solo si lo necesita, con " app_user " |
| D-N01 | DHCP, DNS, Docker | 192.168.1.3 | No |
| CLI-WIN-N01 | Cliente Windows | 192.168.1.100 | Solo pruebas, con " lectura " |
| CLI-LNX-N01 | Cliente Linux | 192.168.1.101 | Solo pruebas, con " lectura " |
| Internet | Exterior | cualquiera | Nunca |

La DMZ es la zona más expuesta. Cada equipo de la DMZ al que se abre el puerto 3306 aumenta el riesgo, por lo que cada servicio debe tener su propia cuenta, su propia regla de firewall y los permisos mínimos.

## 4. Acceso administrativo

### 4.1 Cómo entrar

Por SSH al servidor:

```bash
ssh bchecker@192.168.1.2
```

A MySQL desde el propio servidor (usa el socket, sin `-h`):

```bash
sudo mysql
mysql -u admin_db -p
```

Si el prompt es `bchecker@B-N01:~$` estás en la terminal del sistema. Si es `mysql>` estás dentro de MySQL. Cada sentencia SQL termina en `;`. Si ves el prompt `->`, falta el punto y coma: escríbelo, o cancela con `\c`.

### 4.2 Normas de acceso

- `root``,`` ``ba``c``k``u``p``_``u``ser` y `admin_db` solo se usan desde `localhost`. Nunca de forma remota.
- No escribir contraseñas en la línea de comandos (`-pContraseña`), porque quedan en el historial y en la lista de procesos. Usar `-p` sin valor.
- Los compañeros con acceso SSH deben tener su propio usuario del sistema, sin `sudo`.
- Recordar que `'localhost'` en MySQL significa conexiones desde la propia máquina, incluidas las de quien entre por SSH. La barrera real es la contraseña.

## 5. Servicio MySQL

### 5.1 Archivo de configuración

La configuración del servidor está en:

```
/etc/mysql/mysql.conf.d/mysqld.cnf
```

Atención: el archivo `mysql.cnf` (sin la d final) es del cliente y no sirve para esto.

Parámetros relevantes en la sección `[mysqld]`:

```ini
bind-address = 192.168.1.2
mysqlx-bind-address = 127.0.0.1
```

- `bind-address` indica por qué dirección **propia** escucha el servidor. No filtra qué clientes se conectan: eso lo deciden el firewall, el router y la cuenta de MySQL.
- Con `192.168.1.2` el servidor deja de escuchar en 127.0.0.1:3306. Las conexiones locales normales siguen funcionando por socket, pero `mysql -h 127.0.0.1` fallaría.
- Si la IP de B-N01 cambiara, MySQL no arrancaría correctamente. Por eso la IP debe ser fija.

Tras cualquier cambio se reinicia el servicio.

### 5.2 Gestión del servicio

```bash
sudo systemctl status mysql
sudo systemctl start mysql
sudo systemctl stop mysql
sudo systemctl restart mysql
sudo systemctl enable mysql
```

### 5.3 Comprobar que escucha

```bash
sudo ss -tlnp | grep 3306
```

Debe aparecer `192.168.1.2:3306` y `127.0.0.1:33060`, ambos del proceso `mysqld`.

### 5.4 Registros

```bash
sudo tail -n 50 /var/log/mysql/error.log
```

## 6. Usuarios y permisos

### 6.1 Principios

- **Mínimo privilegio:** cada cuenta tiene solo lo que necesita.
- **Una cuenta por servicio:** si una se compromete o se modifica, las demás no se ven afectadas, y en los registros se distingue quién hizo qué.
- **Host concreto:** en MySQL la identidad de una cuenta es nombre más host. `'app_user'@'192.168.2.2'` y `'app_user'@'localhost'` son cuentas distintas, cada una con su contraseña y sus permisos. Cada cuenta admite un único host.
- **Evitar comodines** como `'%'` o `'192.168.2.%'`.
- **Sin `WITH GRANT OPTION`:** la gestión de usuarios y permisos queda reservada a root.

### 6.2 Cuentas del sistema

| Cuenta | Host | Finalidad | Privilegios |
| --- | --- | --- | --- |
| root | localhost | Emergencias y gestión de permisos | Todos |
| admin\_db | localhost | Administración diaria | ALL PRIVILEGES sobre todo, sin GRANT OPTION |
| app\_user | 192.168.2.2/3 | Aplicación web (W-N01) FTP (F-N01) | SELECT, INSERT, UPDATE, DELETE sobre barcelona\_educacio.\* |
| lectura | 192.168.1.101/2 | Consultas y pruebas (opcional) | SELECT sobre barcelona\_educacio.\* |
| backup\_user | localhost | Copias de seguridad con mysqldump | SELECT, LOCK TABLES, SHOW VIEW, EVENT, TRIGGER sobre barcelona\_educacio.\* |

Las cuentas `debian-sys-maint`, `mysql.infoschema`, `mysql.session` y `mysql.sys` las crea la instalación. No se deben borrar ni modificar.

Si la web solo muestra datos y no los modifica, conviene dejar a `app_user` únicamente `SELECT`.

### 6.3 Crear una cuenta

```sql
CREATE USER 'nombre_cualquiera'@'IP_Fija_o_localhost' IDENTIFIED BY '<CONTRASEÑA>';
GRANT SELECT, INSERT, UPDATE, DELETE ON barcelona_educacio.* TO 'nombre_cualquiera'@'IP_Fija_o_localhost';
```

Un usuario recién creado solo tiene el privilegio `USAGE`, que permite conectarse pero no acceder a datos. Los permisos sobre `base.*` se aplican a todas las tablas, presentes y futuras. Los cambios son inmediatos y no hace falta `FLUSH PRIVILEGES`.

### 6.4 Modificar una cuenta

Cambiar la contraseña:

```sql
ALTER USER 'nombre_cualquiera'@'IP_Fija_o_localhost' IDENTIFIED BY '<NUEVA_CONTRASEÑA>';
```

Quitar permisos:

```sql
REVOKE INSERT, UPDATE, DELETE ON barcelona_educacio.* FROM 'nombre_cualquiera'@'IP_Fija_o_localhost';
```

Bloquear o desbloquear una cuenta sin borrarla:

```sql
ALTER USER 'nombre_cualquiera'@'IP_Fija_o_localhost' ACCOUNT LOCK;
ALTER USER 'nombre_cualquiera'@'IP_Fija_o_localhost' ACCOUNT UNLOCK;
```

Eliminar una cuenta:

```sql
DROP USER 'nombre_cualquiera'@'IP_Fija_o_localhost';
```

### 6.5 Advertencia sobre RENAME USER

Cambiar el host con `RENAME USER` hace que la cuenta **deje de existir para la IP anterior**. Si se renombra `app_user` de 192.168.2.2 a 192.168.2.3, la web pierde el acceso y recibe `Access denied`. Si un servicio nuevo necesita acceso, se crea una cuenta nueva en lugar de renombrar la de otro servicio.

### 6.6 Verificar

```sql
SELECT user, host FROM mysql.user;
SHOW GRANTS FOR 'nombre_cualquiera'@'IP_Fija_o_localhost';
```

## 7. Base de datos

### 7.1 Descripción

| Elemento | Valor |
| --- | --- |
| Base de datos | barcelona\_educacio (utf8mb4) |
| Tabla | equipamientos |
| Origen | Open Data BCN, Ajuntament de Barcelona |
| Contenido | Listado de equipamientos educativos de la ciudad (centros de todos los niveles, autoescuelas, formación de adultos, etc.) |
| Formato original | CSV, 40 columnas, unas 2.700 filas |
| Licencia | Creative Commons Attribution 4.0 |

Todas las columnas se han creado como `TEXT`, porque el CSV trae campos vacíos, identificadores con ceros iniciales (por ejemplo `03` o `098005`) y booleanos escritos como `True`, que no encajan con tipos numéricos. Las columnas principales son `name`, `addresses_road_name`, `addresses_district_name`, `geo_epgs_4326_lat` y `geo_epgs_4326_lon`.

Las aplicaciones deben usar el nombre de tabla `equipamientos` y la columna `name`.

### 7.2 Comandos de consulta habituales

```sql
SHOW DATABASES;
USE barcelona_educacio;
SHOW TABLES;
DESCRIBE equipamientos;
SELECT COUNT(*) FROM equipamientos;
SELECT * FROM equipamientos LIMIT 2\G
```

### 7.3 Procedimiento de importación del CSV

`LOAD DATA` no crea la tabla: debe existir antes y sus columnas deben coincidir en número y orden con la cabecera del CSV.

1. **Subir o descargar el archivo** a B-N01 (con `scp` o `wget`).
2. **Comprobar la codificación** con `file -i archivo.csv`. El archivo original del Ajuntament venía en UTF-16LE, que MySQL no admite en `LOAD DATA`, por lo que se convirtió:

```bash
iconv -f UTF-16 -t UTF-8 educacion.csv -o educacion_utf8.csv
```

3. **Eliminar los retornos de carro** de Windows si los hay (el comando `file` los indica como CRLF).
4. **Crear la tabla vacía** con las columnas de la cabecera.
5. **Activar la carga local** en el servidor y entrar con la opción del cliente:

```sql
SET GLOBAL local_infile = 1;
```

```bash
mysql --local-infile=1 -u admin_db -p
```

6. **Importar:**

```sql
USE barcelona_educacio;
LOAD DATA LOCAL INFILE '/home/bchecker/educacion_utf8.csv'
INTO TABLE equipamientos
CHARACTER SET utf8mb4
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES;
```

7. **Verificar** que no hay avisos y que el recuento cuadra con el archivo (`wc -l` menos 1, por la cabecera):

```sql
SHOW WARNINGS LIMIT 20;
SELECT COUNT(*) FROM equipamientos;
```

8. **Desactivar la carga local**, porque permite que el servidor solicite archivos al cliente:

```sql
SET GLOBAL local_infile = 0;
```

Si se repite la carga, hay que vaciar antes la tabla con `TRUNCATE TABLE equipamientos;`, porque `LOAD DATA` añade filas y no sustituye las existentes.

### 7.4 Errores frecuentes en la importación

| Error | Causa | Solución |
| --- | --- | --- |
| 3948 Loading local data is disabled | local\_infile desactivado en el servidor | `SET GLOBAL local_infile = 1;` |
| 2068 LOAD DATA LOCAL INFILE rejected | Sesión del cliente abierta sin la opción | Entrar con `mysql --local-infile=1` |
| 2 File not found | Ruta incorrecta, por ejemplo sin la barra inicial | Usar la ruta absoluta exacta |
| Caracteres raros en tildes | Codificación incorrecta | Convertir a UTF-8 y usar `CHARACTER SET utf8mb4` |
| Recuento doble | Carga repetida | `TRUNCATE TABLE` y repetir |

## 8. Firewall y red

### 8.1 Firewall de B-N01 (ufw)

Comprobar el estado:

```bash
sudo ufw status numbered
```

Si está inactivo, el servidor no filtra nada y la seguridad depende solo de las cuentas de MySQL y del router. Se recomienda activarlo con reglas de una sola IP. **Permitir SSH siempre antes de activar**, para no perder el acceso remoto:

```bash
sudo ufw allow ssh
sudo ufw allow from 192.168.2.2 to any port 3306
sudo ufw allow from 192.168.2.3 to any port 3306
sudo ufw enable
```

La segunda regla es para la web y la tercera para el FTP, esta última solo si el FTP usa la base de datos. Cada regla de ufw debe tener una cuenta de MySQL equivalente, y viceversa.

### 8.2 Router R-N01

Al estar W-N01 y F-N01 en la DMZ, su tráfico hacia B-N01 pasa por el router. R-N01 debe permitir únicamente:

```
192.168.2.2 -> 192.168.1.2  TCP 3306
192.168.2.3 -> 192.168.1.2  TCP 3306   (si el FTP lo necesita)
```

Cualquier otro flujo de la DMZ hacia la intranet debe permanecer bloqueado.

### 8.3 Probar la conectividad desde un cliente

Desde W-N01 o F-N01:

```bash
nc -zv -w 3 192.168.1.2 3306
```

| Resultado | Significado |
| --- | --- |
| succeeded | Hay ruta, el firewall deja pasar y MySQL escucha. Si falla la aplicación, revisar cuenta y configuración. |
| timed out | Algo descarta el tráfico: ufw de B-N01 o router. |
| Connection refused | Llega, pero nada escucha: revisar bind-address y el estado del servicio. |

Para saber si el tráfico llega a B-N01 se puede escuchar mientras se hace la prueba:

```bash
sudo tcpdump -ni any port 3306
```

## 9. Copias de seguridad

### 9.1 Crear la cuenta de backup

```sql
CREATE USER 'backup_user'@'localhost' IDENTIFIED BY '<CONTRASEÑA>';
GRANT SELECT, LOCK TABLES, SHOW VIEW, EVENT, TRIGGER ON barcelona_educacio.* TO 'backup_user'@'localhost';
```

### 9.2 Hacer una copia

```bash
mysqldump -u backup_user -p --single-transaction --no-tablespaces barcelona_educacio > /home/bchecker/backup_barcelona_educacio.sql
```

- `--single-transaction` da una copia consistente sin bloquear la base (tablas InnoDB).
- `--no-tablespaces` evita pedir el permiso global PROCESS en MySQL 8.

Comprobar que se ha generado:

```bash
ls -lh /home/bchecker/backup_barcelona_educacio.sql
```

### 9.3 Restaurar

La restauración requiere escribir, por lo que se hace con `admin_db`:

```bash
mysql -u admin_db -p barcelona_educacio < /home/bchecker/backup_barcelona_educacio.sql
```

Una copia no está verificada hasta que se ha restaurado con éxito. Conviene probarla periódicamente, idealmente en una base de pruebas.

### 9.4 Automatizar sin exponer la contraseña

Crear un archivo de credenciales legible solo por su propietario:

```ini
[client]
user=backup_user
password=<CONTRASEÑA>
```

```bash
chmod 600 /home/bchecker/.backup.cnf
mysqldump --defaults-extra-file=/home/bchecker/.backup.cnf --single-transaction --no-tablespaces barcelona_educacio > backup.sql
```

Para programarlo, añadir la línea correspondiente con `crontab -e`. Guardar las copias fuera de B-N01 (otro equipo o almacenamiento) y limitar su número para no llenar el disco.

## 10. Mantenimiento periódico

| Frecuencia | Tarea | Comando o acción |
| --- | --- | --- |
| Diaria | Comprobar que el servicio está activo | `sudo systemctl status mysql` |
| Diaria | Revisar errores | `sudo tail -n 50 /var/log/mysql/error.log` |
| Semanal | Verificar la copia de seguridad | `ls -lh` sobre la carpeta de copias |
| Semanal | Comprobar el espacio en disco | `df -h` |
| Mensual | Revisar cuentas y permisos | `SELECT user, host FROM mysql.user;` y `SHOW GRANTS` de cada una |
| Mensual | Probar una restauración | Restaurar en una base de pruebas |
| Mensual | Actualizar el sistema | `sudo apt update && sudo apt upgrade` |
| Trimestral | Rotar contraseñas | `ALTER USER ... IDENTIFIED BY ...` |
| Cuando proceda | Revisar reglas de firewall y del router | `sudo ufw status numbered` |

Ver quién está conectado en este momento:

```sql
SELECT user, host FROM information_schema.processlist;
```

## 11. Resolución de problemas

| Síntoma | Causa probable | Qué hacer |
| --- | --- | --- |
| El cliente se queda esperando y acaba en timeout | Firewall o router descartan el tráfico | Revisar `ufw status`, reglas de R-N01 y `tcpdump` |
| Connection refused | MySQL parado o bind-address incorrecto | `systemctl status mysql` y `ss -tlnp` |
| Access denied for user 'x'@'IP' | Cuenta inexistente para esa IP, contraseña errónea o cuenta bloqueada | Verificar con `SELECT user, host FROM mysql.user;` |
| ERROR 1044 o 1142 | Faltan permisos sobre la base o la tabla | `SHOW GRANTS` y completar el `GRANT` |
| ERROR 1049 Unknown database | Nombre de base mal escrito | Usar `barcelona_educacio` |
| Table doesn't exist o Unknown column | La aplicación usa nombres distintos | Usar la tabla `equipamientos` y la columna `name` |
| La aplicación muestra solo un error genérico | La aplicación oculta el detalle | Mirar los registros de la aplicación y de Nginx o PHP |
| mysql -h 127.0.0.1 falla en B-N01 | MySQL ya no escucha en 127.0.0.1:3306 | Conectar sin `-h` (por socket) |
| MySQL no arranca tras un reinicio | La IP de bind-address ha cambiado | Fijar la IP de B-N01 y revisar `mysqld.cnf` |

### Aplicaciones que se conectan

Los datos de conexión que debe usar cada aplicación son:

| Parámetro | Valor |
| --- | --- |
| Host | 192.168.1.2 (no localhost, porque la base está en otra máquina) |
| Puerto | 3306 |
| Base de datos | barcelona\_educacio |
| Usuario | La cuenta de ese servicio (app\_user para la web) |

El servidor web que ejecute código PHP necesita el módulo `php-mysql`. El archivo de configuración con la contraseña debe estar fuera de la carpeta pública y con permisos restringidos (por ejemplo `chmod 640`).

## 12. Lista de comprobación de seguridad

- [ ] `bind-address` apunta a 192.168.1.2 y no a 0.0.0.0
- [ ] La IP de B-N01 es fija o reservada
- [ ] `ufw` está activo, con SSH permitido y una regla por cada IP autorizada
- [ ] Ninguna cuenta usa host `%` salvo justificación documentada
- [ ] `root` y `admin_db` solo existen para `localhost`
- [ ] Ninguna cuenta tiene `WITH GRANT OPTION` sin necesidad
- [ ] Cada servicio usa su propia cuenta, con los permisos mínimos
- [ ] Las contraseñas son robustas y distintas entre cuentas
- [ ] `local_infile` está en 0 cuando no se está importando
- [ ] Los archivos con credenciales tienen permisos 600 o 640
- [ ] Hay una copia de seguridad reciente y probada
- [ ] Los usuarios del sistema con acceso SSH no tienen `sudo` innecesario
- [ ] El puerto 3306 no es accesible desde Internet

## 13. Referencia rápida

| Acción | Comando |
| --- | --- |
| Entrar como administrador | `sudo mysql` |
| Ver bases de datos | `SHOW DATABASES;` |
| Ver tablas | `SHOW TABLES;` |
| Ver estructura de una tabla | `DESCRIBE equipamientos;` |
| Ver cuentas | `SELECT user, host FROM mysql.user;` |
| Ver permisos de una cuenta | `SHOW GRANTS FOR 'usuario'@'host';` |
| Reiniciar el servicio | `sudo systemctl restart mysql` |
| Ver puerto de escucha | `sudo ss -tlnp \| grep 3306` |
| Ver errores | `sudo tail -n 50 /var/log/mysql/error.log` |
| Estado del firewall | `sudo ufw status numbered` |
| Probar puerto desde un cliente | `nc -zv -w 3 192.168.1.2 3306` |
| Copia de seguridad | `mysqldump -u backup_user -p --single-transaction --no-tablespaces barcelona_educacio > copia.sql` |
| Restaurar | `mysql -u admin_db -p barcelona_educacio < copia.sql` |

## Control de versiones del documento

| Versión | Fecha | Autor | Cambios |
| --- | --- | --- | --- |
| 1.0 | 09/11/2026 |  | Versión inicial |







# Instalación de la base de datos

## Especificaciones de la máquina B-N01:

- Ubuntu server  
- 8 CPUs  
- 12GB RAM  
- Disk Bus: Virtio

Tarjetas de red:  
Default  
Wireguard VPN  
ASIXc2-ITB1

## Objetivos:

Crear una base de datos (MySQL)  
Hacer el DHCP con docker (contenedor 1\)  
Hacer el DNS con docker (contenedor 2\)

## Paquetes instalados:

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

