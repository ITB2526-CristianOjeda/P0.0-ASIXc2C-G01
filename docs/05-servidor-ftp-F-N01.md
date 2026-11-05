### **Tarea 2.5.1: Configuración de Credenciales del Usuario bchecker F-N01**

Crear y verificar el usuario bcheker con contraseña bchecker121 en el sistema operativo y servicio en el equipo F-N01.  
<img width="492" height="312" alt="Captura de pantalla de 2026-11-05 16-05-49" src="https://github.com/user-attachments/assets/663a08aa-9a00-4138-90b9-47304b27c315" />


Añadirlo al grupo de **sudo (administrador).**  
<img width="393" height="107" alt="Captura de pantalla de 2026-11-05 16-06-21" src="https://github.com/user-attachments/assets/65b9a09c-ac41-4510-a7ef-349d3d53b8fd" />


Prueba de verificación SSH:  
Para comprobar que el usuario funciona, conectarse localmente por SSH
<img width="654" height="485" alt="Captura de pantalla de 2026-11-05 16-06-53" src="https://github.com/user-attachments/assets/099d6439-b67e-4766-86e7-45942fecfbcf" />

## 

## **Tarea 2.2: Despliegue del Servidor FTP (F-N01)** 

## **Instalar y configurar el servicio de transferencia de archivos FTP en la máquina F-N01**

Utilizaremos vsftpd (Very Secure FTP Daemon), que es el estándar más utilizado, seguro y ligero.

* Instalar el servicio VSFTPD: sudo apt install \-y vsftpd

<img width="619" height="96" alt="Captura de pantalla de 2026-11-05 16-08-20" src="https://github.com/user-attachments/assets/ad2e2926-b54b-4e98-8429-26ea29de232f" />



* Hacer una copia de seguridad de la configuración original:  
  <img width="641" height="56" alt="Captura de pantalla de 2026-11-05 16-12-36" src="https://github.com/user-attachments/assets/6a4ffed3-0226-41b2-8127-f6954f8df2b7" />


* Editar el archivo de configuración: sudo nano /etc/vsftpd.conf.  
  Descomentar y añadir las siguiente líneas:  
  <img width="604" height="259" alt="Captura de pantalla de 2026-11-05 16-13-14" src="https://github.com/user-attachments/assets/081210cd-b087-46e3-b66b-f83bc3e73253" />

  Guardar y Salir.  
    
* Reiniciar el servicio para aplicar los cambios y comprobar el estado:  
  <img width="632" height="309" alt="Captura de pantalla de 2026-11-05 16-13-46" src="https://github.com/user-attachments/assets/597510da-0317-4e14-9a0a-06ec17b9a64a" />

    
* Prueba de verificación FTP con el usuario bchecker:  
  Crear un archivo de prueba en la carpeta del usuario y conectarse: La prueba fue un éxito.  
  <img width="586" height="307" alt="Captura de pantalla de 2026-11-05 16-14-47" src="https://github.com/user-attachments/assets/c1dab188-7e19-4b4e-ab3c-6e3bb2954228" />



### **Tarea 2.2.1: Despliegue SSH al Servidor FTP (F-N01)**

Asegurar que el acceso SSH esté habilitado para administración en el servidor web F-N01. 

* Instalar el servidor OpenSSH:   
  sudo apt update && sudo apt install \-y openssh-server  
  <img width="620" height="99" alt="Captura de pantalla de 2026-11-05 16-15-13" src="https://github.com/user-attachments/assets/add9f1dd-6002-47e9-a431-578d21a65a65" />

* Habilitar e iniciar el servicio:

<img width="753" height="113" alt="Captura de pantalla de 2026-11-05 16-15-45" src="https://github.com/user-attachments/assets/a9bd3e63-4662-4343-af9a-5679c62f7ec3" />


* Verificar que está funcionando:

<img width="753" height="322" alt="Captura de pantalla de 2026-11-05 16-16-20" src="https://github.com/user-attachments/assets/99879b96-c12f-4bcb-bb2a-89d508167ad7" />


