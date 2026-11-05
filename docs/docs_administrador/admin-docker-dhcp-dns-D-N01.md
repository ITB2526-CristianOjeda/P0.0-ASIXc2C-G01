# Manual de Administración: Servidor Docker (D-N01) - DHCP y DNS

Este documento sirve como referencia técnica para la administración, mantenimiento y revisión de los servicios contenerizados (DHCP y DNS) desplegados en la máquina D-N01.

## 1. Datos de Acceso y Red de la Máquina Host

* **Hostname:** D-N01[cite: 97]
* **Ubicación en la red:** Intranet (ASIXc2-ITB1)[cite: 97]
* **Dirección IP Estática:** 192.168.1.3/24[cite: 97]
* **Puerta de Enlace (Gateway):** 192.168.1.1[cite: 97]
* **Usuario de administración:** bchecker[cite: 97]
* **Contraseña:** bchecker121[cite: 97]

## 2. Gestión del Servicio Docker

El servidor utiliza Docker Engine y Docker Compose para levantar los servicios[cite: 98]. 

Para verificar el estado general del demonio de Docker en el sistema operativo:
```bash
sudo systemctl status docker

Todos los archivos de configuración y el archivo orquestador se encuentran en el siguiente directorio de trabajo:
```bash
cd /configs/docker-D-N01

## 3. Comandos Útiles de Mantenimiento

Para gestionar los contenedores (`dhcp_D-N01` y `dns_D-N01`), utiliza los siguientes comandos desde el directorio de trabajo `/configs/docker-D-N01`:

* **Levantar los servicios en segundo plano:**
  ```bash
  sudo docker-compose up -d
