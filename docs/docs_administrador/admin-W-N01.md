# Manual Técnico de Administración: Servidor Web (`W-N01`)

## 1. Rol y Especificaciones de Red
`W-N01` se ubica en la DMZ y ejecuta **Nginx** (elegido frente a Apache por su arquitectura orientada a eventos, alto rendimiento en conexiones concurrentes y menor consumo de recursos) junto con PHP para servir la aplicación web de consulta de centros educativos de Barcelona.

| Parámetro | Configuración |
| :--- | :--- |
| **Hostname** | `W-N01` |
| **Ubicación / Red** | DMZ (`ASIXc2-ITB1a`) |
| **IP / Gateway** | `192.168.2.2/24` — GW: `192.168.2.1` |
| **FQDN interno** | `www.intranet.local` (resuelto por `D-N01` hacia `192.168.2.2`) |
| **Servicios activos** | Nginx (`80/TCP`), PHP (`php-mysql`), OpenSSH (`22/TCP`) |
| **Usuario administrador** | `bchecker` (grupo `sudo`, contraseña documentada en W-N01: `bchecker123`; estándar global: `bchecker121`) |
| **Rutas críticas** | `/var/www/html/`, `/etc/nginx/sites-enabled/` |

---

## 2. Despliegue de Servicios Base (Nginx y OpenSSH)

1. **Habilitación y arranque de Nginx:**
   ```bash
   sudo systemctl enable nginx
   sudo systemctl start nginx
   sudo systemctl status nginx
   ```

2. **Instalación y arranque de OpenSSH para administración remota:**
   ```bash
   sudo apt install openssh-server -y
   sudo systemctl enable ssh
   sudo systemctl start ssh
   ```

3. **Alta del usuario administrativo `bchecker`:**
   ```bash
   sudo adduser bchecker
   sudo usermod -aG sudo bchecker
   ```

---

## 3. Endurecimiento, Permisos y Conector de Base de Datos

Para desplegar la aplicación web de forma segura en `/var/www/html/`, se aplican las siguientes medidas:

1. **Limpieza y asignación de propiedad/permisos:**
   Se elimina la página por defecto de Nginx, se asigna la propiedad al usuario del servicio web (`www-data`), se fijan permisos estándar (`755` directorios, `644` ficheros) y se restringe `db.php` a `640` por contener la configuración de conexión a MySQL:
   ```bash
   sudo rm /var/www/html/index.nginx-debian.html
   sudo chown -R www-data:www-data /var/www/html
   sudo find /var/www/html -type d -exec chmod 755 {} \;
   sudo find /var/www/html -type f -exec chmod 644 {} \;
   sudo chmod 640 /var/www/html/db.php
   ```

2. **Instalación del conector PHP-MySQL:**
   ```bash
   sudo apt install php-mysql
   ```

3. **Bloqueo de acceso HTTP directo a `db.php` en Nginx:**
   En el bloque de servidor habilitado en `/etc/nginx/sites-enabled/`, se impide la descarga o acceso directo externo al fichero de credenciales:
   ```nginx
   location = /db.php {
       deny all;
   }
   ```

4. **Validación de sintaxis y recarga en caliente:**
   ```bash
   ls -l /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl reload nginx
   ```

---

## 4. Arquitectura de la Aplicación Web (`/var/www/html/`)

| Archivo | Función técnica |
| :--- | :--- |
| `db.php` | Gestiona la conexión segura hacia el servidor MySQL externo `B-N01` (`192.168.1.2`). |
| `api.php` | Recibe las peticiones del buscador, ejecuta consultas preparadas sobre la tabla `equipamientos` y devuelve JSON. |
| `index.html` | Estructura HTML5 con encabezado, cuadro de búsqueda y tabla de 5 columnas. |
| `app.js` | Realiza peticiones `fetch` a `api.php`, gestiona el filtrado en tiempo real e inserta datos con `textContent` para prevenir XSS. |
| `styles.css` | Hoja de estilos responsiva para el panel y la tabla. |

---

## 5. Pruebas de Verificación

* **Servicio web:** Comprobación de acceso HTTP desde el cliente Linux (`CLI-LNX-N01`) y el cliente Windows (`CLI-WIN-N01`) tanto por IP (`192.168.2.2`) como por FQDN (`www.intranet.local`).
* **Integración con BBDD:** Verificación de que la aplicación en `W-N01` consulta y muestra correctamente los registros almacenados en `B-N01` (`192.168.1.2`).
* **Administración remota:** Validación de acceso SSH hacia `W-N01` con el usuario `bchecker`.
