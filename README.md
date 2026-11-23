# Proyecto P0.0 - Despliegue de Infraestructura ASIX2

* **Módulo:** 0379 - Projecte intermodular d'administració de sistemes informàtics en xarxa
* **Curso:** 2026-2027
* **Clase:** ASIXC 2nC
* **Grupo:** 01

---

## 👥 Integrantes del Equipo

* **Eduardo Perales**
* **Cristian Ojeda**
* **Sorin Nicoara**

---

## 🎯 Objetivo del Proyecto

El objetivo principal de este proyecto es planificar, desplegar y documentar una infraestructura de red completa para alojar una aplicación web multicapa. Todo el entorno se virtualiza a través de **IsardVDI**.

La infraestructura incluye un router para gestionar las subredes (**DMZ**, **Intranet** y **NAT**), servidores de servicios (**Web**, **Base de Datos MySQL**, **FTP**, **DHCP** y **DNS** alojados en Docker) y equipos cliente (**Windows** y **Linux**) para verificar la conectividad y resolución de nombres. Todos los sistemas cumplen con el requisito de acceso mediante el usuario estándar del proyecto (`bchecker`).

---

## ⚙️ Archivos de Configuración e Infraestructura (`/config`)

En la carpeta `/config` se almacenan los ficheros de despliegue y configuración de los distintos nodos del sistema:

* **Servidor D-N01 (Docker / DHCP / DNS):**
  * [`docker-compose.yml`](./config/D-N01/docker-compose.yml)

---

## 📚 Documentación Técnica (`/docs`)

Toda la documentación técnica del despliegue, configuraciones y capturas de pantalla se encuentra estructurada en la carpeta `/docs`:

### Planificación y Gestión del Proyecto
* [00 - Introducción, Estudio Previo y Metodología Kanban](./docs/00-introduccion-y-gestion.md)

### Sprint 1: Planificación, Diseño y Red
* 📄 **[Resumen Ejecutivo del Sprint 1 (ProofHub y Arquitectura)](./docs/Sprint1/sprint1.md)**
* [01 - Arquitectura de Red y Direccionamiento IP](./docs/Sprint1/01-arquitectura-y-red.md)
* [02 - Despliegue y Configuración del Router (R-N01)](./docs/Sprint1/02-router-R-N01.md)
* [06 - Servidor DHCP y DNS con Docker (D-N01)](./docs/Sprint1/06-servidor-dns-dhcp-D-N01.md)

### Sprint 2: Despliegue de Servidores, Servicios y Base de Datos
* 📄 **[Resumen Ejecutivo del Sprint 2 (ProofHub y Servicios)](./docs/Sprint2/sprint2.md)**
* [03 - Servidor Web (W-N01)](./docs/Sprint2/03-servidor-web-W-N01.md)
* [04 - Servidor de Base de Datos MySQL (B-N01)](./docs/Sprint2/04-servidor-bbdd-B-N01.md)
* [05 - Servidor FTP (F-N01)](./docs/Sprint2/05-servidor-ftp-F-N01.md)

### Sprint 3: Clientes, Aplicación Web, Pruebas y Git
* 📄 **[Resumen Ejecutivo del Sprint 3 (ProofHub y Pruebas Finales)](./docs/Sprint3/sprint3.md)**
* [07 - Cliente Linux (CLI-LNX-N01)](./docs/Sprint3/07-cliente-linux.md)
* [08 - Cliente Windows (CLI-WIN-N01)](./docs/Sprint3/08-cliente-windows.md)
* [09 - Aplicación Web CRUD](./docs/Sprint3/09-aplicacion-web.md)
* [10 - Control de Versiones Git y Seguridad SSH](./docs/Sprint3/10-git-y-seguridad.md)

### Normativa y Seguridad Física
* [11 - Prevención de Riesgos Laborales (PRL)](./docs/11-prevencion-riesgos-laborales.md)

---

## 🚀 Mejoras y Backlog (`/Mejoras_Backlog`)

En la carpeta `/Mejoras_Backlog` se documentan las ampliaciones y optimizaciones aplicadas sobre la infraestructura base (como la resolución de nombres FQDN `www.intranet.local` y salida DNS pública):

* [Ver carpeta de Mejoras del Backlog](./Mejoras_Backlog)

---

## 📖 Manuales Específicos (Administración y Cliente)

### Guías de Administración (`/docs/docs_administrador`)
* [Administración Base de Datos (B-N01)](./docs/docs_administrador)
* [Administración Servidor FTP (F-N01)](./docs/docs_administrador)
* [Administración Docker, DHCP y DNS (D-N01)](./docs/docs_administrador)

### Guías de Cliente (`/docs/docs_cliente`)
* [Guía Rápida para Clientes - Base de Datos (B-N01)](./docs/docs_cliente)

---

*Repositorio creado y gestionado para la entrega de la Práctica P0.0 del Institut Tecnològic Barcelona.*
