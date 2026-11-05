# Manual de Administración: Servidor FTP y SSH (F-N01)

Este documento centraliza los procedimientos de despliegue, configuración y mantenimiento para los servicios de transferencia de archivos (FTP) y administración remota (SSH) en la máquina F-N01.

## 1. Especificaciones del Servidor

* **Hostname:** F-N01
* **Ubicación en la red:** DMZ (`ASIXc2-ITB1a`)
* **Dirección IP Fija:** `192.168.2.3/24`
* **Puerta de Enlace (Gateway):** `192.168.2.1` (Router R-N01)
* **Servicios Activos:** `vsftpd` (Puerto 21), `sshd` (Puerto 22)

---

## 2. Fase 1: Creación del Usuario Administrativo (Tarea 2.5.1)

Para gestionar los servicios y acceder al servidor, se requiere un usuario estándar con privilegios de administración (sudo) y acceso FTP.

### 2.1. Creación del usuario `bchecker`
```bash
sudo adduser bchecker
```
* **Contraseña asignada:** `bchecker121`

### 2.2. Asignación de privilegios de administrador
```bash
sudo usermod -aG sudo bchecker
```

### 2.3. Comprobación de grupos
```bash
groups bchecker
```
*(Debe devolver: `bchecker : bchecker sudo`)*

---

## 3. Fase 2: Instalación y Configuración del Servidor FTP (Tarea 2.2)

Se utiliza `vsftpd` configurado con jaula `chroot` para garantizar que los usuarios locales solo puedan acceder a su propio directorio personal.

### 3.1. Instalación de vsftpd
```bash
sudo apt update && sudo apt install -y vsftpd
```

### 3.2. Copia de seguridad de la configuración original
```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
```

### 3.3. Configuración de `/etc/vsftpd.conf`
Editar el archivo con `sudo nano /etc/vsftpd.conf` y aplicar las siguientes directivas:

```text
# Deshabilitar acceso anónimo
anonymous_enable=NO

# Habilitar acceso a usuarios locales (ej. bchecker)
local_enable=YES

# Permitir subida y modificación de archivos
write_enable=YES

# Enjaular a los usuarios en su directorio raíz (/home/usuario)
chroot_local_user=YES

# Permitir escritura en el directorio enjaulado
allow_writeable_chroot=YES

# Máscara por defecto para archivos nuevos (022 = rw-r--r--)
local_umask=022
```

### 3.4. Reinicio y verificación del servicio
```bash
sudo systemctl restart vsftpd
sudo systemctl status vsftpd
```

### 3.5. Pruebas de validación FTP local
Comprobar que el usuario `bchecker` puede iniciar sesión y listar archivos:
```bash
ftp localhost
```
* **Usuario:** `bchecker`
* **Contraseña:** `bchecker121`
* **Comando de prueba:** Ejecutar `ls` dentro de la sesión FTP para confirmar la lectura del directorio y luego `bye` para salir.

---

## 4. Fase 3: Instalación del Servidor SSH (Tarea 2.2.1)

Para permitir el acceso y la gestión remota del servidor de forma segura, se despliega OpenSSH.

### 4.1. Instalación del paquete
```bash
sudo apt update && sudo apt install -y openssh-server
```

### 4.2. Habilitación e inicio automático
```bash
sudo systemctl enable --now ssh
```

### 4.3. Prueba de validación SSH local
Confirmar que el usuario creado puede establecer conexión por SSH:
```bash
ssh bchecker@localhost
```

---

## 5. Mantenimiento y Auditoría (Operativa Diaria)

Esta sección contiene los comandos necesarios para la administración diaria del sistema.

### 5.1. Gestión de Servicios (Systemd)
```bash
# Iniciar / Detener / Reiniciar FTP
sudo systemctl start vsftpd
sudo systemctl stop vsftpd
sudo systemctl restart vsftpd

# Iniciar / Detener / Reiniciar SSH
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
```

### 5.2. Monitorización de Logs y Conexiones
```bash
# Ver registro en tiempo real de transferencias FTP
sudo tail -f /var/log/vsftpd.log

# Ver logs del sistema para el servicio FTP
sudo journalctl -u vsftpd -n 50 --no-pager

# Ver logs de intentos de inicio de sesión SSH
sudo journalctl -u ssh -n 50 --no-pager

# Comprobar los puertos a la escucha (21 y 22)
sudo ss -tulpn | grep -E ':21|:22'
```
