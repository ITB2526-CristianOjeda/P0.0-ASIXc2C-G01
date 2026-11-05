# Manual de Administración: Servidor Docker (D-N01) - DHCP y DNS

Este documento sirve como referencia técnica para la administración, mantenimiento y revisión de los servicios contenerizados (DHCP y DNS) desplegados en la máquina D-N01.

## 1. Datos de Acceso y Red de la Máquina Host

* **Hostname:** D-N01
* **Ubicación en la red:** Intranet (ASIXc2-ITB1)
* **Dirección IP Estática:** 192.168.1.3/24
* **Puerta de Enlace (Gateway):** 192.168.1.1
* **Usuario de administración:** bchecker
* **Contraseña:** bchecker121

## 2. Gestión del Servicio Docker

El servidor utiliza Docker Engine y Docker Compose para levantar los servicios[cite: 98]. 

Para verificar el estado general del demonio de Docker en el sistema operativo:
```bash
sudo systemctl status docker
```

Todos los archivos de configuración y el archivo orquestador se encuentran en el siguiente directorio de trabajo:
```bash
cd /configs/docker-D-N01
```
## 3. Comandos Útiles de Mantenimiento

Para gestionar los contenedores (`dhcp_D-N01` y `dns_D-N01`), utiliza los siguientes comandos desde el directorio de trabajo `/configs/docker-D-N01`:

* **Levantar los servicios en segundo plano:**
  ```bash
  sudo docker-compose up -d
  ```

  * **Comprobar el estado de los contenedores activos:**
  ```bash
  sudo docker ps
  ```
* **Revisar los registros (logs) del servidor DHCP para auditoría de IPs asignadas:**
  ```bash
  sudo docker logs dhcp_D-N01
  ```
* **Revisar los registros (logs) del servidor DNS:**
  ```bash
  sudo docker logs dns_D-N01
  ```

## 4. Archivos de Configuración Principales

Si es necesario modificar rangos de IPs, añadir nuevas resoluciones de nombres o actualizar las imágenes, se deben editar los siguientes archivos ubicados en `/configs/docker-D-N01`.

### 4.1. docker-compose.yml
Define la infraestructura de los contenedores, configurados en modo `host` para conectar directamente con la tarjeta de red física `enp2s0`.

```yaml
version: 3.8
services:
  #Servicio DHCP
  dhcp-server:
    image: networkboot/dhcpd:latest
    container_name: dhcp_D-N01
    network_mode: host
    restart: always
    environment:
      - INTERFACE=enp2s0
    volumes:
      - ./dhcpd.conf:/data/dhcpd.conf:ro

  #Servicio DNS
  dns-server:
    image: 4km3/dnsmasq:latest
    container_name: dns_D-N01
    network_mode: host
    restart: always
    volumes:
      - ./dnsmasq.conf:/etc/dnsmasq.conf:ro
    cap_add:
      - NET_ADMIN
```

### 4.2. dhcpd.conf (Configuración DHCP)
Gestiona la asignación dinámica de IPs en la Intranet.

```text
default-lease-time 600;
max-lease-time 7200;
authoritative;

subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 192.168.1.3;
  option domain-name "intranet.local";
}
```

### 4.3. dnsmasq.conf (Configuración DNS)
Actúa como DNS de la red, resolviendo nombres locales y redirigiendo peticiones externas a servidores públicos.

```text
#escuchar en la interfaz de red local
listen-address=127.0.0.1,192.168.1.3
bind-interfaces

#desactivar el servicios dhcp interno de dnsmasq (solo actuara como DNS)
no-dhcp-interface=

#no reenviar consultas de la intranet a los DNS publicos
local=/intranet.local/

#redireccionar peticiones externas a DNS publicos (google / cloudflare)
server=8.8.8.8
server=1.1.1.1

#resoluciones locales de nombres
address=/R-N01/192.168.1.1
address=/R/192.168.1.1
address=/R-N01.intranet.local/192.168.1.1
address=/R.intranet.local/192.168.1.1
```
