# 🔬 Laboratorio 1: Tu Primera Aplicación con Argo CD

> **🎯 Objetivo**: Instalar Argo CD, conectar un repositorio y desplegar una aplicación simple, entendiendo los estados `Synced` y `OutOfSync`.

## 📚 Prerrequisitos

- Un clúster de Kubernetes (ej. Kind, Minikube).
- `kubectl` configurado para apuntar a tu clúster.
- Un repositorio Git público en GitHub con una carpeta que contenga manifiestos de Kubernetes. Puedes hacer un fork y usar este: `https://github.com/argoproj/argocd-example-apps`.

---

## ⚙️ Paso 1: Instalar Argo CD

1.  **Crear el namespace para Argo CD:**

    ```bash
    kubectl create namespace argocd
    ```

2.  **Aplicar el manifiesto de instalación oficial:**

    ```bash
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

3.  **Verificar que los pods de Argo CD estén corriendo:**

    ```bash
    kubectl get pods -n argocd
    # Espera a que todos los pods estén en estado 'Running'.
    ```

---

## 🔑 Paso 2: Acceder a la UI de Argo CD

1.  **Exponer el `argocd-server` usando `port-forward`:**

    ```bash
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```
    *Este comando se quedará corriendo en tu terminal.*

2.  **Obtener la contraseña inicial del administrador:**

    La contraseña se almacena en un `Secret`.

    ```bash
    argocd admin initial-password -n argocd
    # O con kubectl:
    # kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
    ```

3.  **Iniciar sesión en la UI:**

    -   Abre tu navegador y ve a `https://localhost:8080`.
    -   Acepta la advertencia de seguridad del certificado autofirmado.
    -   Usa el usuario `admin` y la contraseña que obtuviste.

---

## 🚀 Paso 3: Crear tu Primera Aplicación

Crearemos una aplicación que despliega un `guestbook` desde el repositorio de ejemplos.

1.  **Crear el manifiesto de la aplicación (`guestbook-app.yaml`):**

    ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Application
    metadata:
      name: guestbook
      namespace: argocd
    spec:
      project: default
      source:
        repoURL: https://github.com/argoproj/argocd-example-apps.git
        targetRevision: HEAD
        path: guestbook
      destination:
        server: https://kubernetes.default.svc
        namespace: guestbook
      syncPolicy:
        syncOptions:
        - CreateNamespace=true # Crea el namespace si no existe
    ```

2.  **Aplicar el manifiesto en el clúster:**

    ```bash
    kubectl apply -f guestbook-app.yaml
    ```

3.  **Observar la aplicación en la UI de Argo CD:**

    -   Verás una nueva tarjeta llamada `guestbook`.
    -   Inicialmente, estará en estado `Missing` y `OutOfSync`.

---

## 🔄 Paso 4: Sincronizar la Aplicación

1.  **Sincronizar desde la UI:**
    -   Haz clic en la tarjeta de la aplicación `guestbook`.
    -   Haz clic en el botón `SYNC` en la parte superior.
    -   Deja las opciones por defecto y haz clic en `SYNCHRONIZE`.

2.  **Verificar el resultado:**
    -   Observa cómo Argo CD crea los recursos (Deployments, Services, etc.).
    -   En unos momentos, el estado de la aplicación cambiará a `Synced` y `Healthy`.

3.  **Verificar los recursos en el clúster:**

    ```bash
    kubectl get all -n guestbook
    # Deberías ver el pod y el servicio del guestbook funcionando.
    ```

---

## 💥 Paso 5: Simular un "Drift" (Desviación)

Vamos a simular un cambio manual en el clúster para ver cómo Argo CD lo detecta.

1.  **Eliminar un recurso manualmente:**

    ```bash
    kubectl delete deployment guestbook-ui -n guestbook
    ```

2.  **Observar el estado en Argo CD:**
    -   Vuelve a la UI de Argo CD.
    -   Después de unos momentos (o haciendo clic en `REFRESH`), la aplicación volverá al estado `OutOfSync`.
    -   Argo CD mostrará que el `Deployment` `guestbook-ui` está `Missing`.

3.  **Corregir el "Drift":**
    -   Vuelve a sincronizar la aplicación (`SYNC`).
    -   Argo CD recreará el `Deployment` eliminado, restaurando el estado deseado de Git.

---

## ✅ Conclusión del Laboratorio

¡Felicidades! Has completado el ciclo básico de GitOps con Argo CD:

-   Instalaste y accediste a Argo CD.
-   Definiste una `Application` para que apunte a un repositorio Git.
-   Sincronizaste la aplicación para desplegar los recursos.
-   Detectaste y corregiste una desviación de la configuración (`drift`).