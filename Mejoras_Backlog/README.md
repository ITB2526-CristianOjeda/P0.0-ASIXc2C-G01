# Mejoras del Backlog

A continuación se documentan las mejoras adicionales implementadas en la infraestructura del proyecto:

## Mejora 1: Segmentación y normalización de la BBDD
**Descripción:** Optimizar la base de datos dividiéndola en tablas relacionadas (1-N y N-M).

*(Documentación pendiente de añadir)*

---

## Mejora 2: Modelo CRUD con API REST
**Descripción:** Implementar rutas tipo API REST para la aplicación usando Python-Flask o PHP.

*(Documentación pendiente de añadir)*

---

## Mejora 3: Despliegue de Honeypot en la DMZ
**Descripción:** Implementar un servicio señuelo (Honeypot) en la zona DMZ para registrar intentos de ataque.

*(Documentación pendiente de añadir)*

---

## Mejora 4: Script de despliegue automatizado
**Descripción:** Crear scripts (Bash/Ansible) para automatizar la instalación y configuración de la infraestructura.

*(Documentación pendiente de añadir)*

---

## Mejora 5: Seguridad SSH con Port Knocking y Fail2ban
**Descripción:** Proteger el puerto SSH configurando bloqueo automático de IPs y apertura por secuencia de puertos.

*(Documentación pendiente de añadir)*

---

## Mejora 6: Certificados SSL/TLS para Web y FTP
**Descripción:** Configurar cifrado HTTPS para el servidor web y FTPS para el servidor FTP.

*(Documentación pendiente de añadir)*

---

## Mejora 7: Conexión con DNS y dominios públicos
**Descripción:** Configurar resolución con nombres FQDN / dominios públicos.

Se ha configurado el servidor DNS (**D-N01**) para acceder a la web interna mediante el nombre de dominio **www.intranet.local (IP 192.168.2.2)** y permitir a la vez la salida hacia dominios públicos de internet.

**1. Resolución y acceso a la web interna (www.intranet.local):**

<img width="546" height="223" alt="image" src="https://github.com/user-attachments/assets/0101f039-6dd2-46d0-a50e-8d12064d2193" />

<img width="740" height="356" alt="image" src="https://github.com/user-attachments/assets/c26fc2ed-0d21-4b6d-abb1-aa03fd0d11f3" />


**2. Resolución y acceso a dominios públicos (www.google.com):**

<img width="709" height="502" alt="image" src="https://github.com/user-attachments/assets/661bec31-079c-4846-8722-dc242a027328" />

<img width="600" height="391" alt="image" src="https://github.com/user-attachments/assets/e09a395e-5a8f-44d4-8147-1a8cf9ec24d3" />
