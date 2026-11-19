# Introducción, Estudio Previo y Gestión del Proyecto

## 1. Estudio Previo de Tecnologías

El diseño y la implementación de la infraestructura para este proyecto se fundamentan en un análisis técnico de las alternativas del mercado, priorizando el rendimiento, la seguridad de los sistemas y nuestra experiencia previa con las herramientas seleccionadas.

* **Entorno de Virtualización (IsardVDI):** Se descarta la virtualización local tradicional (como VirtualBox) a favor de IsardVDI para contar con un entorno centralizado y colaborativo sin depender del hardware de los equipos personales, agilizando la configuración de redes internas y enrutamiento.
* **Contenerización de Servicios (Docker para DNS y DHCP):** En lugar de dedicar máquinas virtuales completas, se despliegan contenedores mediante Docker Engine en el servidor `D-N01`. Al compartir el núcleo del sistema anfitrión, se reduce drásticamente el consumo de RAM y CPU y se acelera el despliegue.
* **Servidor de Transferencia de Archivos (vsftpd):** En `F-N01` se elige `vsftpd` (*Very Secure FTP Daemon*) frente a opciones como ProFTPd por criterios de seguridad, robustez y facilidad para enjaular (`chroot`) a los usuarios locales en sus directorios.
* **Servidor Web (Nginx):** Para `W-N01` en la DMZ se opta por Nginx sobre Apache gracias a su arquitectura orientada a eventos, que ofrece un rendimiento superior ante conexiones concurrentes y un menor consumo de recursos.
* **Base de Datos (MySQL):** En `B-N01` (Intranet) se utiliza MySQL Server para almacenar y consultar el dataset de equipamientos educativos de Barcelona, garantizando rapidez, fiabilidad y gestión granular de permisos remotos.

---

## 2. Gestión del Proyecto y Metodología Kanban

Para la planificación, seguimiento y control del proyecto se ha empleado la metodología ágil **Kanban** mediante la plataforma **ProofHub**, estructurando la ejecución en tres Sprints consecutivos.

### 2.1. Reparto de Roles e Infraestructura
* **Cristian Ojeda:** Despliegue y administración del servidor Docker `D-N01` (contenedores DHCP y DNS), servidor FTP `F-N01` (`vsftpd` y SSH) y gestión de Git/SSH.
* **Eduardo Perales:** Configuración del router `R-N01` (interfaces, enrutamiento, NAT y seguridad con Port Knocking) y del servidor Web `W-N01` (Nginx).
* **Sorin Nicoara:** Despliegue del servidor de base de datos `B-N01` (MySQL) e importación del dataset CSV de educación de Barcelona.

### 2.2. Organización del Backlog y Sprints
* **Backlog:** Tareas de mejora y ampliación del proyecto.
* **Sprint 1 (Planificación, Diseño y Red):** Creación del repositorio, definición del árbol de documentación, diagrama de red, despliegue del router `R-N01` y configuración del servidor Docker `D-N01` (DHCP y DNS).
* **Sprint 2 (Despliegue de Servidores, Servicios y Base de Datos):** Instalación y configuración de los servidores Web `W-N01`, FTP `F-N01`, MySQL `B-N01`, carga del dataset CSV e implementación del usuario estándar `bchecker`.
* **Sprint 3 (Clientes, Aplicación Web, Pruebas y Git):** Configuración de clientes Windows y Linux, despliegue de la aplicación web CRUD, pruebas finales, configuración SSH y consolidación de manuales.

### 2.3. Flujo de Trabajo en ProofHub
* **To Do (Pendiente):** Tareas asignadas al responsable del servicio al iniciar el Sprint.
* **In Progress (En curso):** Tareas en fase de ejecución y configuración en IsardVDI.
* **Done (Completado):** Tareas configuradas, validadas mediante pruebas técnicas y finalizadas en el tablero.

<!-- Opcional: Puedes añadir aquí tus capturas del tablero de ProofHub guardándolas en docs/img/ -->
<!-- ![Tablero Sprint 1](./img/proofhub-sprint1.png) -->
