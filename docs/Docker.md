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

```bash
docker run hello-world
```

---

## Instalación de Docker en Windows

[https://docs.docker.com/desktop/setup/install/windows-install/](https://docs.docker.com/desktop/setup/install/windows-install/)

### Requisitos previos
- Windows 10 o superior con soporte para WSL 2.
- Habilitar virtualización en el BIOS.

### Pasos para instalar Docker

1) **Descargar Docker Desktop:**
   
   - Ve al sitio oficial de Docker y descarga Docker Desktop: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

2) **Ejecutar el instalador:**
   
   - Sigue las instrucciones del asistente de instalación.

3) **Habilitar WSL 2:**
   
   - Asegúrate de que WSL 2 está habilitado en tu sistema:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl --set-default-version 2
```

4) **Configurar Docker Desktop:**
   
   - Abre Docker Desktop y sigue las instrucciones para configurar WSL 2.

5) **Verificar la instalación:**
   
   - Abre una terminal y ejecuta:

```powershell
docker --version
```

6) **Probar Docker:**
   
   - Ejecuta el comando:

```powershell
docker run hello-world
```

### Opcional: Configuración adicional

- Habilitar compartir recursos (discos, memoria, CPU) en la configuración de Docker Desktop.
- Instalar herramientas complementarias como Docker Compose.

---

Con estos pasos, Docker debería estar instalado y funcionando correctamente tanto en Ubuntu como en Windows.
