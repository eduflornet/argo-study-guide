# 🔬 Laboratorio 2: Sincronización Avanzada con Argo CD

> **🎯 Objetivo**: Utilizar `AppProjects` para la gobernanza y `Sync Waves` y `Hooks` para orquestar despliegues complejos.

## 📚 Prerrequisitos

-   Haber completado el Laboratorio 1.
-   Argo CD instalado y funcionando.
-   Un repositorio Git donde puedas hacer commits (puedes usar el fork de `argocd-example-apps`).

---

## 🔐 Paso 1: Crear un `AppProject` para Gobernanza

Los `AppProjects` son cruciales para la seguridad. Crearemos un proyecto que restrinja dónde y desde dónde se puede desplegar.

1.  **Definir el manifiesto del `AppProject` (`production-project.yaml`):**

    ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: AppProject
    metadata:
      name: production
      namespace: argocd
    spec:
      description: "Proyecto para aplicaciones de producción"
      # Permitir desplegar solo desde este repositorio
      sourceRepos:
      - 'https://github.com/argoproj/argocd-example-apps.git'
      # Permitir desplegar solo en el namespace 'production' en el clúster local
      destinations:
      - namespace: production
        server: https://kubernetes.default.svc
      # Denegar el despliegue de ciertos tipos de recursos (ej. ClusterRole)
      clusterResourceWhitelist:
      - group: ''
        kind: Namespace
      - group: '*'
        kind: '*'
      clusterResourceBlacklist:
      - group: 'rbac.authorization.k8s.io'
        kind: ClusterRole
      - group: 'rbac.authorization.k8s.io'
        kind: ClusterRoleBinding
    ```

2.  **Aplicar el manifiesto:**

    ```bash
    kubectl apply -f production-project.yaml
    ```

3.  **Crear una aplicación asociada a este proyecto (`helm-app.yaml`):**

    ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Application
    metadata:
      name: helm-guestbook
      namespace: argocd
    spec:
      project: production # <-- Asociado a nuestro nuevo proyecto
      source:
        repoURL: https://github.com/argoproj/argocd-example-apps.git
        targetRevision: HEAD
        path: helm-guestbook
      destination:
        server: https://kubernetes.default.svc
        namespace: production
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
        syncOptions:
        - CreateNamespace=true
    ```

4.  **Aplicar y verificar:**

    ```bash
    kubectl apply -f helm-app.yaml
    ```
    En la UI de Argo CD, verás la nueva aplicación `helm-guestbook` bajo el proyecto `production`.

---

## 🌊 Paso 2: Orquestar con `Sync Waves`

Aseguraremos que un `Namespace` se cree antes que los recursos que contiene.

1.  **Crear una carpeta en tu repositorio Git** con los siguientes dos ficheros:

    -   `namespace.yaml`:
        ```yaml
        apiVersion: v1
        kind: Namespace
        metadata:
          name: wave-test
          annotations:
            argocd.argoproj.io/sync-wave: "-10" # Ola negativa: se aplica primero
        ```

    -   `configmap.yaml`:
        ```yaml
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: my-config
          namespace: wave-test # Depende del namespace
          annotations:
            argocd.argoproj.io/sync-wave: "0" # Ola por defecto
        data:
          message: "Hello from sync wave!"
        ```

2.  **Crea una aplicación en Argo CD** que apunte a esta carpeta.

3.  **Sincroniza y observa:**
    -   Al sincronizar, Argo CD leerá las anotaciones `sync-wave`.
    -   Primero aplicará el `Namespace` (wave -10) y luego el `ConfigMap` (wave 0), evitando errores de "namespace not found".

---

## 🪝 Paso 3: Usar un `PreSync Hook`

Simularemos una migración de base de datos usando un `Job` que debe completarse antes de que la aplicación principal se sincronice.

1.  **Añade este fichero a tu repositorio Git (`presync-job.yaml`):**

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: presync-migration
      annotations:
        argocd.argoproj.io/hook: PreSync # <-- Define este Job como un hook PreSync
        argocd.argoproj.io/hook-delete-policy: HookSucceeded # Elimina el Job si tiene éxito
    spec:
      template:
        spec:
          containers:
          - name: migration
            image: alpine:3.17
            command: ["sh", "-c", "echo 'Running database migration...'; sleep 15; echo 'Migration complete.'"]
          restartPolicy: Never
      backoffLimit: 1
    ```

2.  **Crea una aplicación que apunte a la carpeta** que contiene este `Job` y otros manifiestos de tu aplicación.

3.  **Sincroniza y observa:**
    -   Al hacer clic en `SYNC`, verás que la sincronización entra en estado `Running`.
    -   En la vista de la aplicación, el `Job` `presync-migration` se creará y ejecutará primero.
    -   Los demás recursos de la aplicación no se crearán hasta que el `Job` se complete con éxito.

---

## ✅ Conclusión del Laboratorio

Has aprendido a usar características avanzadas de Argo CD para gestionar despliegues de forma segura y ordenada:

-   **`AppProjects`**: Para definir barreras de seguridad y gobernanza.
-   **`Sync Waves`**: Para controlar el orden de creación de recursos y gestionar dependencias.
-   **`Hooks`**: Para ejecutar lógica personalizada (como `Jobs`) en diferentes fases del ciclo de sincronización.