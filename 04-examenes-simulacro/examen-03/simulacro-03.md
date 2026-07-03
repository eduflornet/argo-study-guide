# 📝 Examen Simulacro 3 - CAPA

> **⏰ Tiempo**: 60 minutos | **Preguntas**: 35 | **Puntaje**: 75% para aprobar

---

## 🎯 **Pregunta 1** (GitOps - Principios)
¿Cuál de los siguientes NO es uno de los cuatro principios fundamentales de GitOps?

A) El estado deseado debe ser declarativo.
B) El estado deseado debe estar versionado en Git.
C) **Los cambios deben ser aplicados manualmente por un operador para mayor seguridad.**
D) Los agentes de software aseguran la correctitud y alertan sobre divergencias.

---

## 🎯 **Pregunta 2** (Argo CD - Sincronización)
¿Cuál es la diferencia entre `Refresh` y `Sync` en la UI de Argo CD?

A) `Refresh` aplica los cambios, mientras que `Sync` solo los muestra.
B) **`Refresh` actualiza el estado de comparación con Git, mientras que `Sync` aplica los cambios al clúster.**
C) `Refresh` es una operación automática y `Sync` es manual.
D) Son sinónimos y realizan la misma acción.

---

## 🎯 **Pregunta 3** (Argo Workflows - Artefactos)
¿Qué es un `Artifact Repository` en Argo Workflows y por qué es importante?

A) Un repositorio Git donde se almacenan los manifiestos de los workflows.
B) Una base de datos interna que guarda el estado de los workflows.
C) **Un almacenamiento externo (ej. S3) para guardar los artefactos de salida, ya que no se almacenan en el clúster.**
D) Un registro de imágenes Docker para los contenedores de los workflows.

---

## 🎯 **Pregunta 4** (Argo Rollouts - Blue/Green)
En una estrategia Blue-Green, ¿qué permite la opción `autoPromotionEnabled: false`?

A) Promueve automáticamente la versión `green` tan pronto como esté lista.
B) **Pausa el `Rollout` después de que la versión `green` esté lista y el `previewService` esté disponible, requiriendo una promoción manual.**
C) Deshabilita por completo la capacidad de promover la nueva versión.
D) Revierte automáticamente la versión `green` si no se promueve en un tiempo determinado.

---

## 🎯 **Pregunta 5** (Argo Events - Componentes)
¿Qué componente de Argo Events es responsable de ejecutar una acción (como lanzar un workflow) cuando se cumple un conjunto de dependencias de eventos?

A) EventSource
B) EventBus
C) **Sensor**
D) Trigger Gateway

---

## 🎯 **Pregunta 6** (Argo CD - Arquitectura)
¿Qué componente de la arquitectura de Argo CD es responsable de aplicar los manifiestos al clúster de Kubernetes?

A) API Server
B) Repository Server
C) **Application Controller**
D) Redis

---

## 🎯 **Pregunta 7** (Argo Workflows - Loops)
¿Para qué se utiliza la construcción `withSequence` en Argo Workflows?

A) Para iterar sobre una lista de strings.
B) Para ejecutar una secuencia de `steps` en un orden definido.
C) **Para ejecutar un `template` un número determinado de veces, pasando un índice numérico en cada iteración.**
D) Para secuenciar la ejecución de múltiples `DAGs`.

---

## 🎯 **Pregunta 8** (Argo CD - Hooks)
¿Qué es un `resource hook` en Argo CD?

A) Un webhook que notifica a sistemas externos sobre el estado de la sincronización.
B) **Un recurso de Kubernetes (como un `Job`) que se ejecuta en diferentes fases del ciclo de sincronización (ej. `PreSync`, `PostSync`).**
C) Un punto de extensión para añadir lógica personalizada al `Application Controller`.
D) Una anotación para que Argo CD ignore un recurso.

---

## 🎯 **Pregunta 9** (Argo Rollouts - Análisis)
¿Qué proveedores de métricas soporta Argo Rollouts para los `AnalysisTemplates`? (Selecciona todas las correctas)

A) **Prometheus**
B) **Datadog**
C) **Kayenta**
D) **Job**

---

## 🎯 **Pregunta 10** (Argo Workflows - DAGs)
¿Qué ventaja principal ofrece un `DAG` sobre un `template` de tipo `steps`?

A) Los `DAGs` son más fáciles de escribir y leer.
B) Los `DAGs` pueden ejecutar tareas en un bucle, mientras que los `steps` no.
C) **Los `DAGs` permiten definir dependencias complejas y no lineales, optimizando el paralelismo.**
D) Los `steps` no pueden pasar artefactos entre tareas, mientras que los `DAGs` sí.

---

## 🎯 **Pregunta 11** (Argo CD - Sincronización)
¿Qué significa el estado `OutOfSync` en una aplicación de Argo CD?

A) La aplicación tiene errores de sintaxis en sus manifiestos.
B) **El estado real de los recursos en el clúster no coincide con el estado deseado definido en Git.**
C) La aplicación está esperando una ventana de sincronización para aplicarse.
D) El repositorio Git no es accesible.

---

## 🎯 **Pregunta 12** (Argo Events - EventBus)
¿Qué implementación de `EventBus` se recomienda para entornos de producción debido a su persistencia y fiabilidad?

A) In-Memory
B) **NATS Streaming**
C) HTTP
D) Kubernetes Events

---

## 🎯 **Pregunta 13** (Argo CD - ApplicationSets)
¿Qué `generator` de `ApplicationSet` usarías para crear aplicaciones basadas en directorios encontrados dentro de un repositorio Git?

A) List Generator
B) Cluster Generator
C) **Git Generator (con `directories`)**
D) Matrix Generator

---

## 🎯 **Pregunta 14** (Argo Workflows - CLI)
¿Cuál es el comando correcto para enviar (ejecutar) un workflow desde un fichero YAML?

A) `argo create workflow.yaml`
B) `argo run workflow.yaml`
C) **`argo submit workflow.yaml`**
D) `argo apply workflow.yaml`

---

## 🎯 **Pregunta 15** (Argo Rollouts - Estrategias)
¿Cómo logra Argo Rollouts dividir el tráfico entre versiones sin modificar directamente el `Service` de Kubernetes?

A) Creando múltiples `Services` y cambiando las etiquetas de los pods.
B) **Integrándose y manipulando recursos de un Ingress Controller (como NGINX) o un Service Mesh (como Istio).**
C) Usando `iptables` directamente en los nodos del clúster.
D) Modificando los `Endpoints` del `Service` a través de la API de Kubernetes.

---

## 🎯 **Pregunta 16** (Argo CD - Helm)
En la especificación de una `Application` de Argo CD, ¿cuál es la sintaxis correcta para especificar un fichero de valores de Helm?

A) `helm.values: "values.yaml"`
B) `helm.valuesFiles: ["values.yaml"]`
C) **`helm.valueFiles: ["values.yaml"]`**
D) `helm.parameters: { file: "values.yaml" }`

---

## 🎯 **Pregunta 17** (Argo Workflows - Scheduling)
En un `CronWorkflow`, ¿qué hace la política `concurrencyPolicy: Forbid`?

A) Permite que múltiples instancias del workflow se ejecuten simultáneamente.
B) Reemplaza una ejecución antigua con una nueva si se superponen.
C) **Previene que se inicie una nueva ejecución del workflow si la anterior todavía está en curso.**
D) Prohíbe la ejecución del workflow por completo.

---

## 🎯 **Pregunta 18** (Argo CD - Sincronización)
¿Para qué se utiliza la opción `ignoreDifferences` en la especificación de una `Application`?

A) Para ignorar errores de sincronización y marcar la aplicación como `Synced`.
B) **Para que Argo CD no marque la aplicación como `OutOfSync` debido a cambios en campos específicos (ej. `replicas` gestionado por un HPA).**
C) Para ignorar ficheros específicos en el repositorio Git.
D) Para que la opción `prune` no elimine ciertos recursos.

---

## 🎯 **Pregunta 19** (Argo Rollouts - Análisis)
¿Qué es `failureLimit` en la definición de una métrica de un `AnalysisTemplate`?

A) El número máximo de `AnalysisRuns` que pueden fallar antes de que el `Rollout` se aborte.
B) **El número de mediciones consecutivas fallidas de una métrica que causarán que el `AnalysisRun` falle.**
C) El límite de tiempo para que un análisis se complete.
D) El umbral de valor que define un fallo.

---

## 🎯 **Pregunta 20** (Argo Workflows - Templates)
¿Qué tipo de `template` de Argo Workflows usarías para ejecutar un script de Python sin tener que construir una imagen de Docker personalizada?

A) `container`
B) **`script`**
C) `python`
D) `resource`

---

## 🎯 **Pregunta 21** (Argo CD - Comandos)
¿Qué comando de Argo CD se utiliza para gestionar los clústeres de destino?

A) `argocd target ...`
B) `argocd destination ...`
C) **`argocd cluster ...`**
D) `argocd context ...`

---

## 🎯 **Pregunta 22** (Argo Events - Triggers)
¿Cuáles de los siguientes son `triggers` válidos que un `Sensor` puede ejecutar? (Selecciona todas las correctas)

A) **Crear un recurso de Kubernetes.**
B) **Iniciar un Argo Workflow.**
C) **Hacer una petición HTTP.**
D) **Publicar un mensaje en una cola de NATS/Kafka.**

---

## 🎯 **Pregunta 23** (Argo Rollouts - Comandos)
¿Qué comando usarías para revertir inmediatamente un `Rollout` a su versión estable anterior?

A) `kubectl argo rollouts revert <rollout-name>`
B) `kubectl argo rollouts rollback <rollout-name>`
C) **`kubectl argo rollouts abort <rollout-name>`**
D) `kubectl argo rollouts undo <rollout-name>`

---

## 🎯 **Pregunta 24** (Argo Workflows - TTL)
¿Cómo se puede configurar un workflow para que sus recursos (Pods, etc.) se eliminen automáticamente 10 minutos después de que haya finalizado?

A) Añadiendo una anotación `argocd.argoproj.io/ttl: "600"`.
B) **Configurando `spec.ttlStrategy.secondsAfterCompletion: 600` en el manifiesto del `Workflow`.**
C) Ejecutando `argo delete --older-than 10m`.
D) Es una configuración global del `workflow-controller` y no se puede definir por workflow.

---

## 🎯 **Pregunta 25** (Argo CD - Kustomize)
Cuando se usa Kustomize con Argo CD, ¿dónde se definen las imágenes a sobreescribir para un entorno específico?

A) En el `ConfigMap` de `argocd-cm`.
B) **En el fichero `kustomization.yaml` del `overlay` correspondiente, usando el campo `images`.**
C) En la especificación de la `Application` de Argo CD, bajo `kustomize.images`.
D) En un `patch` que modifica directamente el campo `image` del `Deployment`.

---

## 🎯 **Pregunta 26** (Argo Rollouts - Pausa)
¿Qué hace la opción `pause: { duration: 5m }` en un `step` de un `Rollout`?

A) Pausa el `Rollout` indefinidamente y requiere promoción manual después de 5 minutos.
B) **Pausa el `Rollout` durante 5 minutos y luego lo promueve automáticamente al siguiente paso si no hay errores.**
C) Detiene el análisis de métricas durante 5 minutos.
D) Escala la nueva versión a 0 réplicas durante 5 minutos.

---

## 🎯 **Pregunta 27** (Argo CD - Seguridad)
¿Qué son las `Sync Windows` en un `AppProject`?

A) Una interfaz gráfica para visualizar las sincronizaciones.
B) **Ventanas de tiempo configurables que permiten o deniegan la sincronización de aplicaciones, útiles para implementar "deployment freezes".**
C) El tiempo máximo que puede durar una sincronización.
D) Un log de todas las sincronizaciones ocurridas en una ventana de tiempo.

---

## 🎯 **Pregunta 28** (Argo Workflows - Entradas)
¿Cuál es el propósito de la sección `spec.entrypoint` en un manifiesto de `Workflow`?

A) Definir el contenedor que actuará como punto de entrada de red.
B) **Especificar cuál de los `templates` definidos en el manifiesto es el punto de inicio del workflow.**
C) Configurar los parámetros de entrada para todo el workflow.
D) Apuntar a un `WorkflowTemplate` externo.

---

## 🎯 **Pregunta 29** (Argo CD - Alto Rendimiento)
¿Por qué el `Application Controller` de Argo CD generalmente se ejecuta con una sola réplica, incluso en configuraciones de alta disponibilidad?

A) Porque consume demasiados recursos para tener más de una réplica.
B) **Porque es un componente con estado (stateful) que mantiene bloqueos sobre las aplicaciones que gestiona, y múltiples réplicas podrían causar conflictos de reconciliación.**
C) Porque la alta disponibilidad se logra a través del `Repository Server`.
D) Por una limitación en su licencia de uso.

---

## 🎯 **Pregunta 30** (Argo Rollouts - Blue/Green)
En una estrategia Blue-Green, ¿para qué sirve el `previewService`?

A) Es el servicio que recibe el tráfico de producción.
B) **Es un servicio interno que apunta a la nueva versión (green) y puede ser usado para pruebas de aceptación antes de la promoción.**
C) Es un servicio que se crea temporalmente para previsualizar los cambios en la UI de Argo Rollouts.
D) Es un alias para el `activeService`.

---

## 🎯 **Pregunta 31** (Argo Workflows - Salidas)
¿Cómo puede un paso de un workflow exponer un valor (ej. un ID generado) para que sea usado por pasos posteriores?

A) Escribiendo el valor en un `ConfigMap` y leyéndolo en el siguiente paso.
B) **Escribiendo el valor en un fichero y declarándolo como un `outputs.parameters` con una `valueFrom.path`.**
C) Imprimiendo el valor en la salida estándar (stdout).
D) Usando `outputs.env` para exponerlo como variable de entorno.

---

## 🎯 **Pregunta 32** (Argo CD - Autenticación)
¿Cuál de los siguientes métodos de autenticación para repositorios Git NO es soportado nativamente por Argo CD?

A) Clave privada SSH.
B) Token de acceso personal HTTPS.
C) Autenticación de GitHub App.
D) **Credenciales de usuario/contraseña sin codificar en la URL.**

---

## 🎯 **Pregunta 33** (Argo Events - Lógica)
¿Cómo se puede configurar un `Sensor` para que se active si ocurre el `eventoA` O el `eventoB`?

A) Definiendo dos `triggers` diferentes, uno para cada evento.
B) Usando `dependencies: [eventoA, eventoB]` con una política `any`.
C) **Definiendo dos `dependencies` separadas en la lista, ya que la lógica por defecto entre ellas es OR.**
D) No es posible, los `Sensors` solo soportan lógica AND entre dependencias.

---

## 🎯 **Pregunta 34** (Argo Rollouts - Métricas)
En un `AnalysisTemplate`, si una consulta a Prometheus devuelve múltiples series temporales, ¿cómo se accede al valor de la primera serie en la `successCondition`?

A) `result.first`
B) `result[1]`
C) **`result[0]`**
D) `result.value`

---

## 🎯 **Pregunta 35** (Argo Workflows - Secretos)
¿Cuál es la forma recomendada de proporcionar un secreto (como una contraseña de base de datos) a un `container` en un `template` de Argo Workflows?

A) Pasándolo como un parámetro de workflow en la línea de comandos.
B) Escribiéndolo directamente en el manifiesto YAML del `Workflow`.
C) **Creando un `Secret` de Kubernetes y referenciándolo a través de `valueFrom.secretKeyRef` en las variables de entorno.**
D) Almacenándolo en un artefacto de un paso anterior.

---

## 🏁 Fin del Examen

### 📊 Distribución de Preguntas:
- **Argo CD**: 14 preguntas (40%)
- **Argo Workflows**: 10 preguntas (29%)
- **Argo Events**: 5 preguntas (14%)
- **Argo Rollouts**: 6 preguntas (17%)

### 🎯 Puntaje:
**27/35 preguntas correctas = 77% (APROBADO)**