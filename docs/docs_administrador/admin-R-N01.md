# Manual Técnico de Administración: Router Perimetral (`R-N01`)

## 1. Rol y Especificaciones de Red
`R-N01` actúa como puerta de enlace y router perimetral sobre IsardVDI, interconectando la red externa (WAN/NAT), la zona desmilitarizada (DMZ) y la red interna (Intranet). Gestiona el enmascaramiento de salida a Internet y protege su acceso SSH externo mediante *Port Knocking*.

| Parámetro | Configuración |
| :--- | :--- |
| **Hostname** | `R-N01` |
| **Interfaz WAN (`enp1s0`)** | Red `Default` — DHCP (IsardVDI) |
| **Interfaz Intranet** | Red `ASIXc2-ITB1` — `192.168.1.1/24` |
| **Interfaz DMZ** | Red `ASIXc2-ITB1a` — `192.168.2.1/24` |
| **Servicios activos** | Enrutamiento IPv4, NAT (`iptables`), `knockd`, SSH |
| **Usuario administrador** | `bchecker` (grupo `sudo`, contraseña documentada en R-N01: `bchecker123`; estándar global: `bchecker121`) |
| **Ficheros críticos** | `/etc/sysctl.d/99-ipforward.conf`, `/etc/knockd.conf`, `/etc/default/knockd` |

---

## 2. Configuración de Enrutamiento y NAT

1. **Activación de IP Forwarding (reenvío IPv4):**
   Se crea el archivo `/etc/sysctl.d/99-ipforward.conf` con el parámetro `net.ipv4.ip_forward=1` para permitir el tráfico entre subredes. Se aplica en caliente sin reiniciar:
   ```bash
   sudo sysctl -p /etc/sysctl.d/99-ipforward.conf
   ```

2. **Enmascaramiento NAT y persistencia de reglas:**
   Para que los equipos de la Intranet y la DMZ salgan a Internet utilizando la IP de la interfaz externa `enp1s0` (realizando IsardVDI un segundo enmascaramiento), se añade la regla `POSTROUTING` y se instala `iptables-persistent` para conservar la configuración tras reinicios:
   ```bash
   sudo iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE
   sudo apt update && sudo apt install iptables-persistent -y
   ```

---

## 3. Seguridad Perimetral: Port Knocking (`knockd`)

Para ocultar el puerto administrativo SSH (`22/TCP`) frente a escaneos desde la WAN, se implementa `knockd`, abriendo el puerto temporalmente solo a la IP que envíe una secuencia específica de paquetes TCP `SYN`.

1. **Instalación:**
   ```bash
   sudo apt install knockd -y
   ```

2. **Configuración (`/etc/knockd.conf`):**
   * **`[options]`**: `UseSyslog` (registra eventos en `/var/log/syslog`) e `Interface = enp1s0` (interfaz WAN de escucha).
   * **`[openSSH]`**: Secuencia `7000,8000,9000` (`seq_timeout = 15`, `tcpflags = syn`). Ejecuta:
     ```bash
     /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
     ```
   * **`[CloseSSH]`**: Secuencia inversa `9000,8000,7000` (`seq_timeout = 15`). Elimina la regla de acceso:
     ```bash
     /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
     ```

3. **Activación del demonio y cierre por defecto de SSH en WAN:**
   Se habilita el servicio editando `/etc/default/knockd`, se reinicia y se descarta silenciosamente (`DROP`) el tráfico entrante al puerto `22/TCP` por `enp1s0`:
   ```bash
   sudo nano /etc/default/knockd
   sudo systemctl restart knockd
   sudo systemctl status knockd
   sudo iptables -A INPUT -i enp1s0 -p tcp --dport 22 -j DROP
   ```

---

## 4. Administración de Usuarios y Verificación

* **Alta de usuario administrativo:**
  ```bash
  sudo adduser bchecker
  sudo usermod -aG sudo bchecker
  ```

* **Pruebas de verificación:**
  * **Conectividad hacia Intranet:** `ping` desde `R-N01` a `B-N01` (`192.168.1.2`) y `D-N01` (`192.168.1.3`).
  * **Enrutamiento entre subredes:** comprobación de conectividad desde la Intranet hacia la DMZ.
  * **Auditoría de *Port Knocking*:** revisión del estado con `systemctl status knockd` y registros en `/var/log/syslog`.
