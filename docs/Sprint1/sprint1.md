# Sprint 1: Planificación, Diseño y Red

## 🎯 Objetivos del Sprint
En esta primera fase del proyecto se han sentado las bases de la infraestructura virtualizada en **IsardVDI**, estableciendo la organización documental en GitHub, el diseño lógico de las subredes y el despliegue de los servicios core de red (enrutamiento, NAT, DHCP y DNS).

* **Gestión y Documentación:** Creación del repositorio `P0.0-ASIXc2C-G01` y estructuración del árbol de documentación en Markdown.
* **Diseño de Red:** Definición del direccionamiento IP para las subredes **Intranet** (`192.168.1.0/24`) y **DMZ** (`192.168.2.0/24`).
* **Enrutamiento y Seguridad Perimetral:** Despliegue del router **R-N01** con soporte NAT (MASQUERADE) y protección SSH mediante *Port Knocking* (`knockd`).
* **Servicios de Red Contenerizados:** Despliegue del servidor **D-N01** con Docker Engine para gestionar la asignación dinámica de IPs (DHCP) y la resolución de nombres local (DNS).

---

## 📊 Seguimiento del Sprint (ProofHub)

A continuación se documenta la evolución del tablero Kanban de nuestro equipo, mostrando la planificación inicial y la consecución de las tareas al cierre del sprint.

### 1. Estado Inicial (Sprint Backlog)
*Captura del tablero con las tareas recién asignadas, estimadas y listas para empezar.*
<img width="648" height="875" alt="image" src="https://github.com/user-attachments/assets/0d993eb2-dbb4-477d-903f-27a30ca078ed" />


### 2. Estado Final (Done)
*Captura del tablero reflejando el cierre de las tareas, cumplimiento de tiempos y objetivos logrados.*
<img width="634" height="890" alt="image" src="https://github.com/user-attachments/assets/431d5cfb-4e6e-480b-ad8d-6a1d9e3bf4b2" />

---

## 🌐 Resumen de Arquitectura y Direccionamiento IP

* **Usuario estándar de administración:** `bchecker` / `bchecker121`
* **Redes IsardVDI:** Default (WAN/NAT), `ASIXc2-ITB1` (Intranet) y `ASIXc2-ITB1a` (DMZ)

| Equipo / Máquina | Hostname | Ubicación | Dirección IP / Máscara | Gateway | Servicios Principales |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Router** | `R-N01` | WAN / Intranet / DMZ | WAN: DHCP<br>Intranet: `192.168.1.1/24`<br>DMZ: `192.168.2.1/24` | IsardVDI | Enrutamiento, NAT, SSH, Port Knocking |
| **Servidor Docker** | `D-N01` | Intranet | `192.168.1.3/24` | `192.168.1.1` | Docker Engine, DHCP (`100-200`), DNS (`dnsmasq`) |
| **Servidor BBDD** | `B-N01` | Intranet | `192.168.1.2/24` | `192.168.1.1` | MySQL Server (preparado en red) |
| **Servidor Web** | `W-N01` | DMZ | `192.168.2.2/24` | `192.168.2.1` | Nginx, Aplicación Web (preparado en red) |
| **Servidor FTP** | `F-N01` | DMZ | `192.168.2.3/24` | `192.168.2.1` | vsftpd (preparado en red) |
| **Clientes** | `CLI-WIN-N01`<br>`CLI-LNX-N01` | Intranet | Dinámica (`192.168.1.100 - 200`) | `192.168.1.1` | Equipos de prueba Windows y Linux |

---

## 📂 Documentación Técnica del Sprint 1

Accede a los manuales detallados paso a paso de cada tarea desarrollada durante este sprint:

1. **[01 - Arquitectura de Red y Direccionamiento IP](./01-arquitectura-y-red.md)**
   * *Tareas 1.1, 1.2 y 1.3:* Creación del repositorio, estructura de carpetas, diagrama de red y tablas de direccionamiento.
2. **[02 - Despliegue y Configuración del Router (R-N01)](./02-router-R-N01.md)**
   * *Tarea 1.4:* Configuración de interfaces Netplan, `ip_forward`, reglas `iptables` (MASQUERADE) y seguridad *Port Knocking*.
3. **[06 - Servidor DHCP y DNS con Docker (D-N01)](./06-servidor-dns-dhcp-D-N01.md)**
   * *Tareas 1.5, 1.5.1 y 1.5.2:* Instalación de Docker, despliegue de contenedores en modo `host`, configuración de `dhcpd.conf` y `dnsmasq.conf` y pruebas de resolución.

---

## 👥 Reparto de Responsabilidades
* **Eduardo Perales:** Despliegue y configuración completa del Router `R-N01` y reglas de firewall.
* **Cristian Ojeda:** Despliegue del servidor `D-N01` y contenedores de red (DHCP y DNS).
* **Sorin Nicoara:** Estructura del repositorio GitHub, documentación base y diseño de arquitectura.
