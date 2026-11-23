# Sprint 2: Despliegue de Servidores, Servicios y Base de Datos

## 🎯 Objetivos del Sprint
En esta segunda fase del proyecto, el enfoque principal ha sido dotar a la infraestructura de los servicios necesarios para alojar la aplicación web multicapa. Se han desplegado y configurado los servidores de backend y frontend en sus respectivas zonas de seguridad (Intranet y DMZ).

* **Servidor de Base de Datos:** Despliegue de **B-N01** en la Intranet con MySQL Server para almacenar de forma segura los datos de la aplicación, restringiendo el acceso únicamente a los equipos autorizados.
* **Servidor Web:** Implementación de **W-N01** en la DMZ (con Nginx/Apache) para servir la interfaz de la aplicación web de cara al exterior.
* **Servidor FTP:** Configuración de **F-N01** en la DMZ mediante `vsftpd` para permitir la transferencia segura de archivos y la subida de los recursos de la web.
* **Políticas de Seguridad y Usuarios:** Creación de usuarios con privilegios limitados (usuario estándar `bchecker`), configuración de permisos de directorios y reglas de firewall locales.

---

## 📊 Seguimiento del Sprint (ProofHub)

A continuación se documenta la evolución del tablero Kanban de nuestro equipo durante la fase de despliegue de servicios.

### 1. Estado Inicial (Sprint Backlog)
*Captura del tablero con las tareas de configuración de B-N01, W-N01 y F-N01 recién asignadas y estimadas.*
<img width="772" height="894" alt="image" src="https://github.com/user-attachments/assets/beb099f0-0737-40f1-b050-6457f4d31fee" />

### 2. Estado Final (Done)
*Captura del tablero reflejando el cierre de las tareas, resolución de bloqueos y validación de los servicios.*
<img width="586" height="892" alt="image" src="https://github.com/user-attachments/assets/64a29712-f24f-471f-a1fb-8d28d0a333d7" />

---

## 🌐 Resumen de Servicios Desplegados

En este sprint se han levantado los siguientes nodos dentro de IsardVDI, manteniendo las credenciales estándar (`bchecker` / `bchecker121`):

| Equipo / Máquina | Hostname | Ubicación | Dirección IP / Máscara | Gateway | Rol Principal |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Servidor BBDD** | `B-N01` | Intranet | `192.168.1.2/24` | `192.168.1.1` | Motor MySQL, Base de datos de la App Web |
| **Servidor Web** | `W-N01` | DMZ | `192.168.2.2/24` | `192.168.2.1` | Servidor Web (Nginx/Apache), PHP |
| **Servidor FTP** | `F-N01` | DMZ | `192.168.2.3/24` | `192.168.2.1` | Servidor vsftpd, Gestión de archivos |

---

## 📂 Documentación Técnica del Sprint 2

Accede a los manuales detallados paso a paso de cada servidor desarrollado durante este sprint:

1. **[03 - Servidor Web (W-N01)](./03-servidor-web-W-N01.md)**
   * Instalación del servicio web, configuración de Virtual Hosts, despliegue del entorno para la aplicación web y apertura de puertos (80/443).
2. **[04 - Servidor de Base de Datos MySQL (B-N01)](./04-servidor-bbdd-B-N01.md)**
   * Instalación de MySQL Server, configuración del `bind-address`, creación de la base de datos del proyecto y asignación de usuarios y privilegios.
3. **[05 - Servidor FTP (F-N01)](./05-servidor-ftp-F-N01.md)**
   * Despliegue de `vsftpd`, enjaulado de usuarios (chroot), configuración de puertos pasivos/activos y pruebas de subida/bajada de ficheros.

---

## 👥 Reparto de Responsabilidades
* **Eduardo Perales:** Despliegue, configuración y securización del Servidor Web (`W-N01`).
* **Cristian Ojeda:** Despliegue y configuración del Servidor FTP (`F-N01`) y gestión de transferencias.
* **Sorin Nicoara:** Instalación del motor MySQL en el Servidor de Base de Datos (`B-N01`) y preparación del entorno de datos.
