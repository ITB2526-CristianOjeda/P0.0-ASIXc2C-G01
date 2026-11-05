### **Tarea 2.5.1: Configuración de Credenciales del Usuario bchecker F-N01**

Crear y verificar el usuario bcheker con contraseña bchecker121 en el sistema operativo y servicio en el equipo F-N01.  
![][image1]

Añadirlo al grupo de **sudo (administrador).**  
**![][image2]**

Prueba de verificación SSH:  
Para comprobar que el usuario funciona, conectarse localmente por SSH

## 

## **Tarea 2.2: Despliegue del Servidor FTP (F-N01)** 

## **Instalar y configurar el servicio de transferencia de archivos FTP en la máquina F-N01**

Utilizaremos vsftpd (Very Secure FTP Daemon), que es el estándar más utilizado, seguro y ligero.

* Instalar el servicio VSFTPD: sudo apt install \-y vsftpd

![][image3]

* Hacer una copia de seguridad de la configuración original:  
  ![][image4]

* Editar el archivo de configuración: sudo nano /etc/vsftpd.conf.  
  Descomentar y añadir las siguiente líneas:  
  ![][image5]  
  Guardar y Salir.  
    
* Reiniciar el servicio para aplicar los cambios y comprobar el estado:  
  ![][image6]  
    
* Prueba de verificación FTP con el usuario bchecker:  
  Crear un archivo de prueba en la carpeta del usuario y conectarse: La prueba fue un éxito.  
  ![][image7]


### **Tarea 2.2.1: Despliegue SSH al Servidor FTP (F-N01)**

Asegurar que el acceso SSH esté habilitado para administración en el servidor web F-N01. 

* Instalar el servidor OpenSSH:   
  sudo apt update && sudo apt install \-y openssh-server  
  ![][image8]  
* Habilitar e iniciar el servicio:

![][image9]

* Verificar que está funcionando:

![][image10]

[image1]
