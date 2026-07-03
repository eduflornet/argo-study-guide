# 🔬 Laboratorio 3: Construir un Pipeline de CI con Argo Workflows

> **🎯 Objetivo**: Crear un pipeline de CI/CD simple usando un `DAG` en Argo Workflows que clona un repositorio, construye una imagen de Docker y la publica.

## 📚 Prerrequisitos

-   Argo Workflows instalado en tu clúster.
-   Un `Artifact Repository` configurado (ej. MinIO).
-   Credenciales para un registro de contenedores (ej. Docker Hub) almacenadas en un `Secret` de Kubernetes.

---

## ⚙️ Paso 1: Instalar Argo Workflows

Si aún no lo has hecho, instala el controlador de Argo Workflows.

```bash
kubectl create namespace argo
kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo-workflows/stable/manifests/install.yaml
```

> **Nota**: Para este laboratorio, se asume que tienes un registro de artefactos y credenciales de Docker configurados. Si no, puedes adaptar el workflow para omitir los pasos que los requieran.

---

## 🏗️ Paso 2: Definir el `Workflow` de CI

Crearemos un `Workflow` con una estructura de `DAG` (Grafo Acíclico Dirigido) que define tres tareas:

1.  `clone`: Clona un repositorio Git.
2.  `build`: Construye una imagen de Docker usando Kaniko (no requiere un demonio de Docker).
3.  `publish`: Publica la imagen en un registro.

**Crea el fichero `ci-workflow.yaml`:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: ci-pipeline-
spec:
  entrypoint: ci-dag
  serviceAccountName: argo-workflow # Asegúrate de que este SA tenga los permisos necesarios
  arguments:
    parameters:
    - name: repo-url
      value: https://github.com/argoproj/argo-workflows.git
    - name: image-name
      value: your-docker-hub-username/argo-ci-example:latest

  templates:
  - name: ci-dag
    dag:
      tasks:
      - name: clone
        template: clone-repo
        arguments:
          parameters: [{name: repo-url, value: "{{workflow.parameters.repo-url}}"}]

      - name: build-and-push
        template: build-kaniko
        dependencies: [clone] # <-- Esta tarea depende de 'clone'
        arguments:
          parameters: [{name: image-name, value: "{{workflow.parameters.image-name}}"}]

  - name: clone-repo
    inputs:
      parameters:
      - name: repo-url
    container:
      image: alpine/git:latest
      command: [git, clone]
      args: ["{{inputs.parameters.repo-url}}", /src]
    outputs:
      artifacts:
      - name: source-code
        path: /src

  - name: build-kaniko
    inputs:
      parameters:
      - name: image-name
      artifacts:
      - name: source-code
        path: /workspace/
    container:
      image: gcr.io/kaniko-project/executor:v1.9.0
      args:
        - "--dockerfile=/workspace/Dockerfile"
        - "--context=dir:///workspace/"
        - "--destination={{inputs.parameters.image-name}}"
      # Este volumen monta las credenciales de Docker Hub desde un Secret
      # volumeMounts:
      # - name: docker-config
      #   mountPath: /kaniko/.docker/
  # volumes:
  # - name: docker-config
  #   secret:
  #     secretName: docker-hub-credentials
```

---

## 🚀 Paso 3: Ejecutar y Monitorear el Workflow

1.  **Enviar el workflow para su ejecución:**

    ```bash
    argo submit ci-workflow.yaml -n argo --watch
    ```
    El flag `--watch` te permitirá ver el progreso en tiempo real desde la terminal.

2.  **Monitorear desde la UI de Argo Workflows:**
    -   Expón la UI: `kubectl -n argo port-forward svc/argo-server 8081:2746`
    -   Abre `https://localhost:8081`.
    -   Navega a `Workflows` y busca tu pipeline.
    -   Podrás ver el `DAG`, el estado de cada tarea, los logs y los artefactos generados.

---

## ✅ Conclusión del Laboratorio

Has construido un pipeline de CI funcional con Argo Workflows:

-   Usaste un `DAG` para definir dependencias entre tareas.
-   Pasaste `parámetros` al workflow.
-   Compartiste `artefactos` (el código fuente) entre pasos.
-   Integraste herramientas externas como `git` y `Kaniko` en contenedores.