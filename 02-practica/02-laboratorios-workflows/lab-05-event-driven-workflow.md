# 🔬 Laboratorio 5: Automatización con Argo Events

> **🎯 Objetivo**: Configurar un `EventSource` para escuchar webhooks y un `Sensor` para triggerear un `Argo Workflow` en respuesta.

## 📚 Prerrequisitos

-   Argo Events y Argo Workflows instalados.
-   `ngrok` o una herramienta similar para exponer un servicio local a internet.

---

## ⚙️ Paso 1: Instalar Argo Events

1.  **Crear el namespace e instalar los componentes:**

    ```bash
    kubectl create namespace argo-events
    kubectl apply -n argo-events -f https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install.yaml
    ```

2.  **Instalar el `EventBus` (transporte de eventos):**

    Por defecto, se usa una implementación `NATS Streaming`.

    ```bash
    kubectl apply -n argo-events -f https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install-eventbus.yaml
    ```

---

## 🪝 Paso 2: Crear la `EventSource`

La `EventSource` define el punto de entrada para los eventos. Crearemos una que escuche peticiones HTTP en un endpoint.

1.  **Crear el fichero `webhook-eventsource.yaml`:**

    ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: EventSource
    metadata:
      name: webhook
      namespace: argo-events
    spec:
      service:
        ports:
          - port: 12000
            targetPort: 12000
      webhook:
        # Define un endpoint llamado 'example'
        example:
          port: "12000"
          endpoint: /example
          method: "POST"
    ```

2.  **Aplicar el manifiesto:**

    ```bash
    kubectl apply -f webhook-eventsource.yaml -n argo-events
    ```
    Argo Events creará un `Service` y un `Pod` para esta `EventSource`.

---

## 🧠 Paso 3: Crear el `Sensor`

El `Sensor` escucha los eventos del `EventBus` y ejecuta `triggers` cuando se cumplen las dependencias.

1.  **Crear el fichero `webhook-sensor.yaml`:**

    ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Sensor
    metadata:
      name: webhook
      namespace: argo-events
    spec:
      template:
        serviceAccountName: argo-events-sa # SA con permisos para crear workflows
      dependencies:
        - name: test-dep
          eventSourceName: webhook
          eventName: example
      triggers:
        - template:
            name: webhook-workflow-trigger
            argoWorkflow:
              operation: submit
              source:
                resource:
                  apiVersion: argoproj.io/v1alpha1
                  kind: Workflow
                  metadata:
                    generateName: webhook-triggered-workflow-
                  spec:
                    entrypoint: whalesay
                    arguments:
                      parameters:
                      # El payload del webhook se inyecta aquí
                      - name: message
                        value: "Hello from webhook!"
                    templates:
                      - name: whalesay
                        inputs:
                          parameters:
                            - name: message
                        container:
                          image: docker/whalesay
                          command: [cowsay]
                          args: ["{{inputs.parameters.message}}"]
    ```

2.  **Aplicar el manifiesto:**

    ```bash
    kubectl apply -f webhook-sensor.yaml -n argo-events
    ```

---

## 🚀 Paso 4: Probar el Flujo de Eventos

1.  **Exponer el servicio de la `EventSource`:**

    En una terminal, expón el servicio a tu máquina local.

    ```bash
    kubectl port-forward -n argo-events svc/webhook-eventsource-svc 12000:12000
    ```

2.  **Enviar un evento con `curl`:**

    En otra terminal, envía una petición POST al endpoint.

    ```bash
    curl -X POST -H "Content-Type: application/json" -d '{"message":"Hello World"}' http://localhost:12000/example
    ```

3.  **Verificar el resultado:**
    -   El `EventSource` recibe la petición y la publica en el `EventBus`.
    -   El `Sensor` detecta el evento y ejecuta el `trigger`.
    -   Se crea una nueva instancia de `Argo Workflow`.

4.  **Comprobar que el workflow se ha ejecutado:**

    ```bash
    argo list -n argo-events
    # Deberías ver un workflow con el nombre 'webhook-triggered-workflow-...'

    argo logs <nombre-del-workflow> -n argo-events
    # Deberías ver la salida de 'cowsay' con el mensaje.
    ```

---

## ✅ Conclusión del Laboratorio

Has creado un sistema completo de automatización basada en eventos:

-   Configuraste una `EventSource` para recibir eventos externos.
-   Definiste un `Sensor` con una dependencia y un `trigger`.
-   Lograste que un evento HTTP lanzara automáticamente un `Argo Workflow`.
-   Aprendiste a inyectar datos del evento como parámetros en el workflow.