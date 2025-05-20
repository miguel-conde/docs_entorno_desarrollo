# Entorno de Desarrollo WSL + VS Code

Este documento describe el entorno de desarrollo configurado en Windows WSL (Ubuntu) con Visual Studio Code, incluyendo Python, R, Quarto, Docker y herramientas adicionales para Data Science, Machine Learning e IA.

---

## 1. Requisitos Previos

- Windows 10/11 con WSL 2 habilitado.
- VS Code instalado en Windows.
- Docker Desktop instalado y con integración WSL habilitada.

---

## 2. Configuración de WSL

1. **Instalar Ubuntu**

```powershell
   wsl --install -d Ubuntu
````

2. **Actualizar WSL 2**

```powershell
   wsl --set-version Ubuntu 2
```
3. **Iniciar Ubuntu WSL**

   * Desde menú Inicio → "Ubuntu".
   * Actualizar paquetes:

     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

---

## 3. VS Code + Remote-WSL

1. Instalar extensión **Remote - WSL** en VS Code.
2. Abrir una ventana WSL:

   * `F1` → *Remote-WSL: New Window*.
3. Instalar extensiones dentro de WSL (por ejemplo, Python, R, Docker, Quarto). Seleccionar **Install in WSL: Ubuntu**.

---

## 4. Configuración de Git (LF)

Configurar finales de línea a LF globalmente:

```bash
git config --global core.autocrlf false
git config --global core.eol lf
git config --global core.safecrlf true
```

Agregar al proyecto `.gitattributes`:

```gitattributes
* text=auto eol=lf
*.sh text eol=lf
*.py text eol=lf
*.R  text eol=lf
*.md text eol=lf
```

---

## 5. Python

1. **pyenv** (versiones de Python):

   ```bash
   curl https://pyenv.run | bash
   ```
2. Añadir a `~/.bashrc`:

   ```bash
   export PATH="$HOME/.pyenv/bin:$PATH"
   eval "$(pyenv init --path)"
   eval "$(pyenv virtualenv-init -)"
   ```
3. Instalar Python 3.11.4:

   ```bash
   pyenv install 3.11.4
   pyenv global 3.11.4
   ```
4. **pipx** y **Poetry**:

   ```bash
   sudo apt install python3-pip pipx
   pipx ensurepath
   pipx install poetry
   ```
5. Herramientas CLI (aisladas con pipx):

   ```bash
   pipx install black isort flake8 mypy pre-commit jupyterlab dvc cookiecutter uv
   ```

---

## 6. R

1. Instalar R desde repositorio Ubuntu o CRAN `noble-cran40`:

   ```bash
   sudo add-apt-repository universe
   sudo apt update
   sudo apt install r-base r-base-dev
   ```
2. Dependencias del sistema:

   ```bash
   sudo apt install -y libxml2-dev libcurl4-openssl-dev libssl-dev \
     libcairo2-dev libxt-dev libgtk2.0-dev build-essential
   ```
3. Paquetes R esenciales:

   ```r
   install.packages(c("xml2","lintr","roxygen2","httpuv","languageserver"),
                    repos="https://cloud.r-project.org")
   ```
4. **httpgd** desde GitHub:

   ```r
   install.packages("remotes")
   remotes::install_github("nx10/httpgd")
   ```

---

## 7. Quarto

1. Descargar e instalar CLI:

   ```bash
   wget https://github.com/quarto-dev/quarto-cli/releases/download/v1.7.31/quarto-1.7.31-linux-amd64.deb
   sudo apt install ./quarto-1.7.31-linux-amd64.deb
   ```
2. Instalar extensión **Quarto** en WSL.
3. Probar un proyecto Quarto:

   ```bash
   mkdir ~/quarto-ejemplo && cd ~/quarto-ejemplo
   quarto create-project .
   quarto render index.qmd
   ```

---

## 8. Docker

1. Añadir usuario al grupo `docker`:

   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```
2. Verificar:

   ```bash
   docker version
   docker run hello-world
   ```
3. Instalar extensión **Docker** en VS Code (WSL).

---

## 9. Flujo de Trabajo

### Crear proyectos

* **Python**: `poetry new mi_proyecto`; seleccionar intérprete `.venv/bin/python`.
* **R**: `usethis::create_project("miRproyecto")`.

### Abrir proyectos

* Desde VS Code Remote-WSL: `File > Open Folder > ~/projects/...`.
* Para proyectos en Windows: `File > Open Folder > /mnt/c/...`.

### Workspaces

* `File > Add Folder to Workspace...` para múltiples proyectos.
* `File > Save Workspace As...` para crear `.code-workspace`.

### Copiar proyectos

* `cp -r /mnt/c/Users/... ~/projects/`  (o usar `rsync`, `\wsl$` en Explorer)

---

## 10. GitFlow

Instala **git-flow** para gestionar tus ramas siguiendo el modelo GitFlow:

```bash
sudo apt update
sudo apt install git-flow
```

### Iniciar GitFlow en un proyecto

Dentro de la carpeta de tu repositorio Git, ejecuta:

```bash
git flow init
```

Se te preguntará por los nombres de las ramas:

* **Branch master**: rama de producción (por defecto `master` o `main`).
* **Branch develop**: rama de desarrollo (por defecto `develop`).
* **Feature branches prefix**: `feature/`
* **Release branches prefix**: `release/`
* **Hotfix branches prefix**: `hotfix/`
* **Support branches prefix**: `support/`
* **Version tag prefix**: (dejar en blanco o usar `v`)

Puedes aceptar los valores por defecto pulsando **Enter**.

Una vez inicializado, para:

* Crear una nueva **feature**:

  ```bash
  git flow feature start mi-nueva-funcionalidad
  ```
* Finalizar la **feature**:

  ```bash
  git flow feature finish mi-nueva-funcionalidad
  ```
* Crear y finalizar **release** y **hotfix** de forma similar.

---

## 11. Verificaciones Finales

* `git config --list | grep core` (LF)
* `python --version`, `poetry --version`, `pipx list`
* En R: `library(languageserver); library(httpgd)`.
* `quarto --version`
* `docker version`

---

¡Este entorno está listo para desarrollar proyectos de Data Science, Machine Learning e IA en Python y R usando WSL y VS Code!


