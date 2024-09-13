# SRE-challenge-infra

Este repositorio contiene la infraestructura y workflows de CI para la aplicación voting-app.

# SRE-challenge-infra

Este repositorio contiene la infraestructura y los workflows de Integración Continua (CI) para la aplicación de votación (voting-app).

## Estructura del Repositorio

[La estructura del repositorio permanece igual]

## Requerimientos Previos

1. **Git**: Instalado localmente

2. **DockerHub**:
   - Cuenta en Docker Hub para almacenar las imágenes.

3. **Acceso a GitHub**:
   - Cuenta de GitHub con acceso a este repositorio.
   - Permisos para crear y gestionar secretos en el repositorio.

4. **Secretos de GitHub configurados**:
   - `DOCKER_HUB_USERNAME`: Tu nombre de usuario de Docker Hub.
   - `DOCKER_HUB_ACCESS_TOKEN`: Tu token de acceso a Docker Hub.
   - `K8S_ACCESS_TOKEN`: Token de acceso para el repositorio de manifiestos de Kubernetes.

5. **Acceso al repositorio de manifiestos**:
   - Permisos de escritura en el repositorio `camesa/voting-app-k8s` para actualizar los manifiestos.

## Workflows de CI

### 1. Build y Push de Imágenes Docker (ci.yml)

Este workflow se activa en cada push o pull request a la rama `main`.

Funcionalidad:
- Crea imagenes Docker para los servicios `result`, `vote`, y `worker`.
- Publica las imagenes en Docker Hub con los tags `latest` y una versión incremental.

Etapas principales:
1. Checkout del código
2. Configuración de Docker Buildx
3. Login en Docker Hub
4. Generación de versión
5. Build y Push de imágenes Docker
6. Creación y push de tag de Git
7. Print de la nueva versión

### 2. Actualización de Manifiestos de Kubernetes (update-manifests.yml)

Este workflow se activa manualmente (workflow_dispatch) y requiere input del usuario de la versión deseada.

Funcionalidad:
- Actualiza los manifiestos de Kubernetes en un repositorio separado con la nueva versión de las imágenes.

Etapas principales:
1. Checkout del repositorio de manifiestos
2. Actualización de los archivos de manifiesto
3. Commit y push de los cambios

## Uso

### CI Automático
El workflow de CI se ejecutará automáticamente en cada push o pull request a la rama `main`. Esto construirá y publicará las nuevas imágenes Docker para todos los servicios.

### Actualización Manual de Manifiestos
Para actualizar los manifiestos de Kubernetes:
1. Ir a la pestaña "Actions" en GitHub
2. Seleccionar el workflow "Actualizar Manifiestos de Kubernetes"
3. Hacer clic en "Run workflow"
4. Ingresar la versión que queremos utilizar
5. Hacer clic en "Run workflow"

La actualización de manifiestos con el nuevo tag desplegará automáticamente los cambios a través de ArgoCD.
Para conocer más detalles de la etapa de CD de este proyecto ir al repositorio: https://github.com/camesa/voting-app-k8s
