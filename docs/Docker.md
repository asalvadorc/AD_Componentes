# Docker

Docker es una herramienta que utiliza la tecnología de contenedores para empaquetar aplicaciones y sus dependencias en unidades portátiles y ligeras. Esto garantiza que una aplicación se ejecute de la misma manera en cualquier entorno, ya sea en una computadora local, un servidor de desarrollo, o en la nube.

**Conceptos clave de Docker**{.azul}

- **Contenedores**:

Son entornos aislados que contienen todo lo necesario para ejecutar una aplicación: código, bibliotecas, dependencias, y configuraciones.
A diferencia de las máquinas virtuales, los contenedores comparten el mismo núcleo del sistema operativo, lo que los hace más ligeros y eficientes.

- **Imágenes**:

Son plantillas inmutables utilizadas para crear contenedores.
Las imágenes son versiones preconfiguradas de un software o aplicación que incluyen todo lo necesario para ejecutarse.

- **Docker Engine**:

Es el motor que ejecuta y gestiona los contenedores.
Permite construir imágenes, iniciar contenedores y comunicarse con el hardware del sistema.

- **Docker Hub**:

Es un repositorio en línea donde se pueden almacenar y compartir imágenes de Docker.
Ofrece una amplia variedad de imágenes predefinidas listas para usar.

**¿Cómo funciona Docker?**{.azul}

- **Construcción de una imagen**:

Los desarrolladores crean un archivo llamado Dockerfile, donde se especifican los pasos para construir la imagen de una aplicación.
A partir del Dockerfile, Docker genera una imagen.

- **Ejecución de un contenedor**:

Usando una imagen, Docker inicia un contenedor que ejecuta la aplicación empaquetada.

- **Distribución de imágenes**:

Las imágenes pueden ser subidas a Docker Hub u otros registros privados para compartirlas y utilizarlas en diferentes sistemas.

Para saber más podéis consultar su página Web: [https://www.docker.com/](https://www.docker.com/)

## Instalación de Docker en Ubuntu

[https://docs.docker.com/desktop/setup/install/linux/ubuntu/](https://docs.docker.com/desktop/setup/install/linux/ubuntu/)

### Requisitos previos
- Asegúrate de que tu sistema está actualizado:

```bash
sudo apt update && sudo apt upgrade -y
```

- Desinstala versiones antiguas de Docker si están instaladas:

```bash
sudo apt remove docker docker-engine docker.io containerd runc
```

### Pasos para instalar Docker

1) **Instalar paquetes necesarios:**

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

2) **Agregar la clave GPG de Docker:**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

3) **Agregar el repositorio de Docker:**

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

4) **Actualizar el índice de paquetes e instalar Docker:**

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

5) **Verificar la instalación:**

```bash
docker --version
```

6) **Habilitar Docker para que inicie con el sistema:**

```bash
sudo systemctl enable docker
```

7) **Opcional: Permitir ejecutar Docker sin `sudo`:**

```bash
sudo usermod -aG docker $USER
# Sal y vuelve a iniciar sesión para aplicar los cambios
```

### Probar Docker
Ejecuta el siguiente comando para probar si Docker está funcionando correctamente:

```
    docker run hello-world
```

---

## Instalación de Docker en Windows

Documentación oficial: [https://docs.docker.com/desktop/setup/install/windows-install/](https://docs.docker.com/desktop/setup/install/windows-install/)


Docker Desktop utiliza **WSL2** internamente para ejecutar contenedores Linux en Windows.  
👉 **No es necesario instalar Ubuntu** si solo se quiere trabajar con **Docker y Docker Compose**.


### Requisitos previos

- Windows 10 (versión 2004 o superior) o Windows 11  
- Cuenta con permisos de **administrador**  
- Virtualización activada en BIOS (normalmente ya lo está)


### Activar WSL2


Abrir un Terminal  y ejecutar los siguientes comandos para habilitar WSL2 y la plataforma de máquina virtual:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
⚠️ Reiniciar Windows tras ejecutar los comandos.

### Instalar Docker Desktop

1) Descargar Docker Desktop desde la web oficial: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)    
2) Ejecutar el instalador: Sigue las instrucciones del asistente de instalación.  

✔ Marcar la opción **'Use WSL 2 based engine'** durante la instalación

### Comprobaciones

Para comprobar que **WSL2** y **Docker** funcionan correctamente, ejecutar en el Terminal:

```powershell
   wsl --list --verbose
   docker --version
   docker compose version
```
👉 Si aparece la distribución 'docker-desktop' con VERSION 2 y Docker responde correctamente, la instalación es correcta.

### Probar Docker

Ejecuta el siguiente comando para probar si Docker está funcionando correctamente:

    docker run hello-world

### Usar Docker Compose

1) Crea una carpeta de proyecto en Windows, por ejemplo:

      C:\docker\postgres-bds\


2) Dentro mete tu archivo:

      docker-compose.yml

3) Desde el Terminal ve a la carpeta donde está el archivo _docker-compose.yml_ y ejecuta:


      docker compose up -d

### Comandos que necesitas

- **Ver contenedores:**
```powershell
   docker ps
```
- **Parar:**
```powershell
   docker compose down
```
- **Borrar también volúmenes (OJO borra datos):**
```powershell
   docker compose down -v
```