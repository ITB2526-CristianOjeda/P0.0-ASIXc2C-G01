# Control de Versiones con Git y Autenticación Segura mediante SSH

Este documento detalla la configuración del par de llaves SSH (Ed25519) para la comunicación segura con GitHub sin contraseñas interactivas, así como el flujo de trabajo de comandos Git utilizado durante el proyecto.

---

## 1. Integración y Seguridad SSH con GitHub

### 1.1. Paso 1: Configuración de la identidad de Git
Antes de generar las llaves, se configura la identidad global en la terminal:

```bash
git config --global user.name "Tu Nombre o Usuario de GitHub"
git config --global user.email "tu_correo_de_github@ejemplo.com"
```

### 1.2. Paso 2: Generación del par de claves SSH (Ed25519)
Se genera un par de claves con el algoritmo Ed25519 sin contraseña (*passphrase* en blanco para automatizar la autenticación):

```bash
ssh-keygen -t ed25519 -C "tu_correo_de_github@ejemplo.com"
```
* **Ruta por defecto:** `~/.ssh/id_ed25519`

### 1.3. Paso 3: Obtención de la clave pública
Se muestra y copia el contenido de la clave pública generada:

```bash
cat ~/.ssh/id_ed25519.pub
```

### 1.4. Paso 4: Vinculación en GitHub
1. Iniciar sesión en GitHub -> **Settings** -> **SSH and GPG keys**.
2. Hacer clic en **New SSH key**.
3. Asignar un título descriptivo (ej: `VM Isard D-N01`) y pegar la clave pública.

---

## 2. Pruebas y Evidencias de Conexión SSH (Tarea 3.3)

### 2.1. Comprobación de autenticación con GitHub
Se verifica la conectividad desde la terminal mediante el siguiente comando:

```bash
ssh -T git@github.com
```

**Resultado esperado:**
> `Hi username! You've successfully authenticated, but GitHub does not provide shell access.`

![Captura de pantalla de la prueba exitosa ssh -T](./img/ssh-test.png)

### 2.2. Configuración del remoto SSH en el repositorio
Cambio de la URL del origen remoto del proyecto para utilizar el protocolo SSH:

```bash
# Cambiar la URL remota a SSH
git remote set-url origin git@github.com:ITB2526-CristianOjeda/P0.0-ASIXc2C-G01.git

# Verificar que el origen apunta a SSH
git remote -v
```

---

## 3. Flujo de Trabajo y Comandos Git (Tarea 3.4)

Para la gestión de cambios y subida de documentación al repositorio central, se sigue el siguiente flujo de comandos:

### 3.1. Descarga e inicio del repositorio
```bash
# Clonar el repositorio mediante SSH
git clone git@github.com:ITB2526-CristianOjeda/P0.0-ASIXc2C-G01.git
```

### 3.2. Sincronización y actualización de cambios
```bash
# Consultar el estado de los archivos modificados
git status

# Añadir los cambios al área de preparación (staging)
git add .

# Guardar los cambios localmente con un mensaje descriptivo
git commit -m "docs: añadir documentación del Sprint 3 y evidencias SSH"

# Enviar los cambios al repositorio remoto en GitHub
git push origin main

# Descargar las últimas actualizaciones del equipo
git pull origin main
```

![Captura de pantalla ejecutando git push sin pedir contraseña](./img/git-push.png)
