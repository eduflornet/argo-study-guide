# 🔬 Laboratorio 4: Despliegue Canary con Argo Rollouts

> **🎯 Objetivo**: Realizar un despliegue Canary, enviando gradualmente tráfico a una nueva versión y usando un análisis automatizado para validar su estabilidad.

## 📚 Prerrequisitos

-   Argo Rollouts instalado en tu clúster.
-   Un Ingress Controller (como NGINX) o un Service Mesh (como Istio) para la manipulación del tráfico. Este laboratorio usará un `Service` como proveedor de métricas para simplificar.

---

## ⚙️ Paso 1: Instalar Argo Rollouts

1.  **Crear el namespace e instalar el controlador:**

    ```bash
    kubectl create namespace argo-rollouts
    kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
    ```

2.  **Instalar el plugin de `kubectl` para Argo Rollouts:**

    Sigue las instrucciones oficiales para tu sistema operativo.

---

##  canary Paso 2: Definir los Recursos del `Rollout`

Crearemos un `Rollout` que gestiona dos versiones de una aplicación, una estable (`blue`) y una candidata (`green`).

1.  **Crear el fichero `rollout-canary.yaml`:**

    ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Rollout
    metadata:
      name: rollouts-demo
    spec:
      replicas: 5
      selector:
        matchLabels:
          app: rollouts-demo
      template:
        metadata:
          labels:
            app: rollouts-demo
        spec:
          containers:
          - name: rollouts-demo
            image: argoproj/rollouts-demo:blue # Versión inicial
            ports:
            - containerPort: 8080
      strategy:
        canary:
          steps:
          - setWeight: 20 # 1. Enviar 20% del tráfico a la nueva versión
          - pause: { duration: 30s } # 2. Pausar por 30 segundos para recolectar métricas
          - setWeight: 50 # 3. Incrementar al 50%
          - pause: {} # 4. Pausa indefinida, esperando promoción manual
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: rollouts-demo
    spec:
      ports:
      - port: 80
        targetPort: 8080
        protocol: TCP
        name: http
      selector:
        app: rollouts-demo
    ```

2.  **Aplicar los manifiestos:**

    ```bash
    kubectl apply -f rollout-canary.yaml
    ```

3.  **Observar el `Rollout`:**

    Usa el dashboard de Argo Rollouts para visualizar el estado.

    ```bash
    kubectl argo rollouts dashboard
    ```
    Verás 5 réplicas de la versión `blue` funcionando.

---

## 🚀 Paso 3: Iniciar el Despliegue Canary

1.  **Actualizar la imagen del `Rollout` a la nueva versión (`green`):**

    ```bash
    kubectl argo rollouts set image rollouts-demo rollouts-demo=argoproj/rollouts-demo:green
    ```

2.  **Monitorear el progreso:**

    ```bash
    kubectl argo rollouts get rollout rollouts-demo --watch
    ```
    -   Verás que Argo Rollouts crea un nuevo `ReplicaSet` para la versión `green`.
    -   El `Rollout` seguirá los `steps` definidos:
        1.  Escalará la versión `green` y le asignará un 20% del peso.
        2.  Pausará por 30 segundos.
        3.  Incrementará el peso al 50%.
        4.  Finalmente, entrará en estado `Paused` esperando intervención manual.

---

## 👍 Paso 4: Promover y Abortar el `Rollout`

1.  **Promover el `Rollout`:**

    Una vez que has verificado que la versión `green` es estable, puedes promoverla para que reciba el 100% del tráfico.

    ```bash
    kubectl argo rollouts promote rollouts-demo
    ```
    El `Rollout` continuará, escalará la versión `blue` a cero y la `green` al 100% de las réplicas.

2.  **(Opcional) Abortar un `Rollout`:**

    Si durante la pausa detectas un problema, puedes abortar el despliegue y revertir a la versión estable.

    ```bash
    # (Ejecutar esto durante una pausa)
    kubectl argo rollouts abort rollouts-demo
    ```
    Esto revertirá inmediatamente todo el tráfico a la versión `blue`.

---

## ✅ Conclusión del Laboratorio

Has implementado un despliegue Canary controlado:

-   Definiste una estrategia `Canary` con `steps` para controlar el peso del tráfico y las pausas.
-   Iniciaste un despliegue actualizando la imagen del `Rollout`.
-   Usaste `kubectl argo rollouts promote` para completar el despliegue y `abort` para revertirlo.