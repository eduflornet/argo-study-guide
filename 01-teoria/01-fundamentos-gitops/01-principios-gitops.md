# 📜 Principios Fundamentales de GitOps

GitOps es un paradigma para la gestión de infraestructura y aplicaciones donde Git es la **única fuente de la verdad**. El estado deseado de todo el sistema se describe de forma declarativa en un repositorio Git.

Se basa en cuatro principios clave:

### 1. El Sistema Completo se Describe Declarativamente

Toda la configuración del sistema (aplicaciones, infraestructura, políticas) debe estar definida en un formato declarativo. En el mundo de Kubernetes, esto se traduce en ficheros **YAML** que describen los recursos (Deployments, Services, ConfigMaps, etc.).

-   **Imperativo vs. Declarativo**:
    -   **Imperativo**: "Ejecuta el comando X, luego el comando Y" (ej. `kubectl run`, `kubectl expose`).
    -   **Declarativo**: "Quiero que el estado final sea este" (ej. `kubectl apply -f manifest.yaml`).

Argo CD opera sobre manifiestos declarativos.

### 2. El Estado Deseado del Sistema está Versionado en Git

Git es el lugar central y único donde se almacena el estado deseado. Cualquier cambio en el sistema debe originarse con un `commit` en el repositorio Git.

-   **Trazabilidad**: Cada cambio queda registrado, con autor, fecha y descripción.
-   **Auditoría**: Es fácil revisar el historial de cambios.
-   **Rollbacks**: Revertir a un estado anterior es tan simple como hacer un `git revert`.

### 3. Los Cambios Aprobados se Aplican Automáticamente al Sistema

Un agente de software (como el **Application Controller** de Argo CD) se encarga de aplicar los cambios del repositorio Git al clúster. Este proceso debe ser automatizado.

-   **Modelo Pull (Recomendado)**: El agente dentro del clúster "tira" (pull) de los cambios desde Git. Es más seguro porque no requiere exponer la API de Kubernetes al exterior. Argo CD sigue este modelo.
-   **Modelo Push**: Un sistema de CI/CD externo "empuja" (push) los cambios al clúster. Requiere que el sistema de CI tenga credenciales de acceso al clúster.

### 4. Agentes de Software Aseguran la Correctitud y Alertan sobre Divergencias

El agente de software no solo aplica los cambios, sino que también **reconcilia continuamente** el estado real del clúster con el estado deseado en Git.

-   **Detección de Drift (Desviación)**: Si alguien aplica un cambio manual en el clúster con `kubectl`, el agente lo detecta y marca el estado como `OutOfSync`.
-   **Auto-reparación (Self-Healing)**: Si se configura, el agente puede revertir automáticamente cualquier cambio no autorizado, asegurando que Git siga siendo la fuente de la verdad.

---

## 🌊 Flujo de Trabajo Típico en GitOps

1.  **Desarrollador**: Hace un cambio en el código de la aplicación y lo sube a un repositorio de código.
2.  **Pipeline de CI**: Se activa, ejecuta pruebas, construye una nueva imagen de Docker y la publica en un registro (ej. Docker Hub).
3.  **Actualización de Configuración**: El pipeline de CI (o un desarrollador) actualiza el manifiesto de despliegue en el **repositorio de configuración de GitOps**, cambiando el tag de la imagen a la nueva versión.
4.  **Merge a `main`**: El cambio se aprueba y se fusiona a la rama principal del repositorio de configuración.
5.  **Argo CD**: Detecta el cambio en el repositorio de configuración.
6.  **Sincronización**: Argo CD "tira" de los nuevos manifiestos y los aplica al clúster de Kubernetes.
7.  **Reconciliación**: Kubernetes se encarga de llevar el sistema al nuevo estado deseado (ej. creando un nuevo Pod con la nueva imagen).

!Flujo GitOps