# Sprint 3: Clientes, Aplicación Web, Pruebas y Git

## 🎯 Objetivos del Sprint
En esta tercera y última fase del proyecto se ha completado la integración end-to-end de la infraestructura desplegada en **IsardVDI**. Se han configurado los equipos cliente para validar la asignación dinámica de red y la resolución de nombres, se ha puesto en producción la aplicación web conectada a la base de datos y se ha estandarizado el flujo de control de versiones mediante SSH.
* **Despliegue de Equipos Cliente:** Configuración de los clientes **Linux** (`CLI-LNX-N01`) y **Windows** (`CLI-WIN-N01`) en la Intranet con obtención automática de IP por DHCP (`192.168.1.100 - 200`) y resolución DNS hacia `192.168.1.3`.
* **Aplicación Web Multicapa (CRUD):** Desarrollo y despliegue en `W-N01` del directorio de centros educativos de Barcelona (`index.html`, `styles.css`, `app.js`, `api.php` y `db.php`), conectando de forma segura con la base de datos `barcelona_educacio` en `B-N01`.
* **Control de Versiones y Seguridad:** Generación de pares de claves SSH (`ed25519`), vinculación con GitHub y estandarización del flujo de trabajo Git (`add`, `commit`, `push`, `pull`, `clone`) para todo el equipo.
* **Validación y Pruebas Finales:** Verificación de conectividad entre subredes, resolución de nombres locales (`www.intranet.local`) y salida a Internet.

---

## 📊 Seguimiento del Sprint (ProofHub)

A continuación se documenta la evolución del tablero Kanban de nuestro equipo durante la fase de despliegue de clientes, desarrollo web y cierre del proyecto.

### 1. Estado Inicial (Sprint Backlog)
*Captura del tablero con las tareas de configuración de clientes, desarrollo de la aplicación web e integración Git/SSH recién asignadas.*
<img width="811" height="892" alt="image" src="https://github.com/user-attachments/assets/d6b5a5f6-9232-43cc-8f3f-3c9867488f90" />

### 2. Estado Final (Done)
*Captura del tablero reflejando el cierre completo de todas las tareas del proyecto y la validación final del sistema.*
<img width="591" height="892" alt="image" src="https://github.com/user-attachments/assets/991f2f58-1c72-4e6e-9b79-6ca65ce19d45" />

---

## 🌐 Resumen de Nodos y Entorno Final

En este sprint se han validado los equipos cliente dentro de la Intranet (`ASIXc2-ITB1`), verificando su comunicación con los servidores de la DMZ y la Intranet:

| Equipo / Servicio | Hostname / URL | Ubicación | Dirección IP / Asignación | Gateway | Función Principal |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Cliente Linux** | `CLI-LNX-N01` | Intranet | Dinámica DHCP (`192.168.1.102/24`) | `192.168.1.1` | Pruebas de red, SSH, cliente web y DNS |
| **Cliente Windows** | `CLI-WIN-N01` | Intranet | Dinámica DHCP (`192.168.1.103/24`) | `192.168.1.1` | Pruebas de red, SSH, cliente web y DNS |
| **Aplicación Web** | `http://192.168.2.2`<br>`http://www.intranet.local` | DMZ (`W-N01`) | `192.168.2.2/24` | `192.168.2.1` | Consulta en tiempo real a MySQL (`B-N01`) |

---

## 📂 Documentación Técnica del Sprint 3

Accede a los manuales detallados paso a paso de cada tarea desarrollada durante este sprint:

1. **[07 - Cliente Linux (CLI-LNX-N01)](./07-cliente-linux.md)**
   * *Tarea 3.1.2:* Configuración de red dinámica por DHCP, verificación de `resolvectl status` y acceso web al directorio educativo.
2. **[08 - Cliente Windows (CLI-WIN-N01)](./08-cliente-windows.md)**
   * *Tarea 3.1.1:* Verificación de concesión DHCP (`ipconfig`), sufijo DNS `intranet.local` y pruebas de navegación.
3. **[09 - Aplicación Web CRUD](./09-aplicacion-web.md)**
   * *Tareas 3.2 y 3.2.1:* Estructura del proyecto en `/var/www/html/`, conexión PDO segura (`db.php`), API JSON (`api.php`), filtrado dinámico en frontend (`app.js`) y protección de rutas en Nginx.
4. **[10 - Control de Versiones Git y Seguridad SSH](./10-git-y-seguridad.md)**
   * *Tareas 3.3 y 3.4:* Generación de claves SSH Ed25519, autenticación contra GitHub (`ssh -T git@github.com`) y guía de comandos Git.

---

## 👥 Reparto de Responsabilidades
* **Eduardo Perales:** Configuración del entorno web en `W-N01` (Nginx, PHP, permisos y seguridad) y pruebas de conectividad.
* **Cristian Ojeda:** Configuración de autenticación SSH con GitHub, guía de control de versiones Git y subida de archivos de infraestructura.
* **Sorin Nicoara:** Integración de la aplicación web con MySQL (`B-N01`), validación de clientes y consolidación final del repositorio.
