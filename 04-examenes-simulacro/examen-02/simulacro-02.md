# 📝 Examen Simulacro 2 - CAPA

> **⏰ Tiempo**: 60 minutos | **Preguntas**: 35 | **Puntaje**: 75% para aprobar

---

## 🎯 **Pregunta 1** (Argo CD - Comandos)
¿Qué hace el comando `argocd app wait <app-name>`?

A) Pausa la sincronización de una aplicación.
B) **Espera a que una aplicación alcance un estado `Synced` y `Healthy`.**
C) Muestra los logs de la aplicación en tiempo real.
D) Elimina la aplicación después de un período de espera.

---

## 🎯 **Pregunta 2** (Argo Workflows - Templates)
¿Qué permite hacer un `WorkflowTemplate` en Argo Workflows?

A) Ejecutar un único script de un solo uso.
B) Definir variables de entorno globales para todos los workflows.
C) **Definir una plantilla de workflow reutilizable que puede ser invocada por otros workflows.**
D) Almacenar los resultados y artefactos de los workflows.

---

## 🎯 **Pregunta 3** (Argo Rollouts - Estrategias)
¿Cuál es la principal diferencia entre un `Rollout` de Argo Rollouts y un `Deployment` de Kubernetes?

A) `Rollout` solo soporta la estrategia `Recreate`.
B) `Rollout` no puede gestionar `ReplicaSets`.
C) **`Rollout` soporta estrategias avanzadas como Canary y Blue-Green, mientras que `Deployment` no.**
D) `Rollout` es un recurso obsoleto y ha sido reemplazado por `Deployment`.

---

## 🎯 **Pregunta 4** (Argo Events - Componentes)
En Argo Events, ¿qué componente es responsable de escuchar eventos externos (como webhooks, mensajes de colas o cambios en S3) y publicarlos en el EventBus?

A) Sensor
B) Trigger
C) **EventSource**
D) EventBus

---

## 🎯 **Pregunta 5** (Argo CD - Sincronización)
¿Cuál es el propósito principal de las `sync waves` en Argo CD?

A) Sincronizar aplicaciones a través de múltiples clústeres simultáneamente.
B) **Controlar el orden de aplicación de los recursos durante una sincronización.**
C) Limitar la cantidad de recursos que se sincronizan al mismo tiempo.
D) Programar sincronizaciones en ventanas de tiempo específicas.

---

## 🎯 **Pregunta 6** (Argo Workflows - DAGs)
En un `DAG` de Argo Workflows, ¿cómo se define que una tarea `C` debe ejecutarse solo después de que las tareas `A` y `B` hayan finalizado con éxito?

A) Usando `after: [A, B]` en la tarea `C`.
B) Definiendo `C` en un `step` posterior a `A` y `B`.
C) **Usando `dependencies: [A, B]` en la definición de la tarea `C`.**
D) Mediante el uso de `withItems: [A, B]`.

---

## 🎯 **Pregunta 7** (Argo CD - AppProjects)
¿Cuál es el propósito principal de un `AppProject` en Argo CD?

A) Agrupar aplicaciones visualmente en la UI.
B) **Proporcionar límites de seguridad y gobernanza, restringiendo repositorios, clústeres y recursos.**
C) Acelerar el tiempo de sincronización de las aplicaciones.
D) Definir plantillas para crear nuevas aplicaciones.

---

## 🎯 **Pregunta 8** (Argo Rollouts - Análisis)
¿Qué es un `AnalysisTemplate` en Argo Rollouts?

A) Una plantilla para definir la estrategia de `Rollout` (Canary o Blue-Green).
B) Un `Job` de Kubernetes que se ejecuta antes de una promoción.
C) **Una plantilla reutilizable que define las métricas y condiciones para evaluar el éxito de una nueva versión.**
D) Un `ConfigMap` que almacena los resultados del análisis.

---

## 🎯 **Pregunta 9** (Argo Workflows - Artefactos)
¿Cómo se pasa un fichero generado en un paso de un workflow a otro paso?

A) Montando un `hostPath` volume en ambos pods.
B) **Usando `outputs.artifacts` en el paso de origen y `inputs.artifacts` en el paso de destino.**
C) Pasando el contenido del fichero como un `parameter`.
D) Los pasos no pueden compartir ficheros, solo parámetros.

---

## 🎯 **Pregunta 10** (Argo CD - Kustomize)
¿Cómo utiliza Argo CD los `overlays` de Kustomize para gestionar múltiples entornos?

A) Creando un `AppProject` diferente para cada `overlay`.
B) **Apuntando el `path` de la `Application` al directorio del `overlay` correspondiente (ej. `overlays/production`).**
C) Usando `helm.valueFiles` para especificar el `overlay`.
D) Los `overlays` no son compatibles con Argo CD, solo la `base`.

---

## 🎯 **Pregunta 11** (Argo Events - Triggers)
¿Qué recurso de Argo Events se utiliza para definir una o más acciones (como iniciar un Argo Workflow) en respuesta a un conjunto de dependencias de eventos?

A) EventSource
B) **Sensor**
C) EventFlow
D) WorkflowTrigger

---

## 🎯 **Pregunta 12** (Argo Workflows - Loops)
¿Cuál es la diferencia clave entre `withItems` y `withParam` en Argo Workflows?

A) `withItems` es para listas de números, `withParam` es para listas de strings.
B) `withItems` ejecuta los pasos en secuencia, `withParam` en paralelo.
C) **`withItems` itera sobre una lista estática, mientras que `withParam` itera sobre una lista JSON generada dinámicamente por un paso anterior.**
D) `withParam` está obsoleto y ha sido reemplazado por `withItems`.

---

## 🎯 **Pregunta 13** (Argo CD - Sincronización)
¿Qué garantiza la opción `selfHeal` en una `syncPolicy` de Argo CD?

A) Que la aplicación se eliminará y recreará si falla.
B) Que los errores de sincronización se reintentarán automáticamente.
C) **Que cualquier cambio manual en el clúster que cause una desviación (drift) será revertido automáticamente al estado de Git.**
D) Que los secretos se gestionarán de forma segura.

---

## 🎯 **Pregunta 14** (Argo Rollouts - Comandos)
¿Qué comando se utiliza para avanzar manualmente un `Rollout` que está en estado de pausa?

A) `kubectl argo rollouts resume <rollout-name>`
B) `kubectl argo rollouts advance <rollout-name>`
C) **`kubectl argo rollouts promote <rollout-name>`**
D) `kubectl argo rollouts continue <rollout-name>`

---

## 🎯 **Pregunta 15** (Argo Workflows - Scheduling)
¿Qué es un `CronWorkflow` y para qué se utiliza?

A) Un workflow que se ejecuta solo una vez en un momento específico.
B) **Un recurso que permite programar la ejecución periódica de un workflow usando sintaxis de `cron`.**
C) Una política para limitar el número de workflows que se ejecutan simultáneamente.
D) Un `template` para workflows relacionados con el tiempo.

---

## 🎯 **Pregunta 16** (Argo CD - Helm)
¿Cómo maneja Argo CD los secretos de forma segura cuando se utiliza un chart de Helm que los requiere?

A) Almacenando los secretos en texto plano en el fichero `values.yaml` dentro de Git.
B) **Integrándose con herramientas como Sealed Secrets, Vault o SOPS para desencriptar los secretos durante la sincronización.**
C) Pasando los secretos como variables de entorno en el pod del `Application Controller`.
D) Argo CD no puede manejar secretos en charts de Helm.

---

## 🎯 **Pregunta 17** (Argo Rollouts - Blue/Green)
En una estrategia Blue-Green de Argo Rollouts, ¿cuál es el propósito del `activeService`?

A) Es el servicio que apunta a la nueva versión (green) para pruebas.
B) **Es el servicio que siempre apunta a la versión estable que recibe el 100% del tráfico de producción.**
C) Es un servicio que balancea el tráfico entre la versión blue y la green.
D) Es el servicio que se elimina después de una promoción exitosa.

---

## 🎯 **Pregunta 18** (Argo CD - Arquitectura)
¿Cuál es el rol del `Repository Server` en la arquitectura de Argo CD?

A) Aplicar los manifiestos al clúster de Kubernetes.
B) Servir la API y la interfaz de usuario de Argo CD.
C) **Clonar repositorios Git, generar manifiestos (usando Helm/Kustomize) y mantener una caché.**
D) Almacenar el estado de las aplicaciones y las sesiones de usuario.

---

## 🎯 **Pregunta 19** (Argo Workflows - Templates)
¿Qué tipo de `template` de Argo Workflows usarías para aplicar un manifiesto de un recurso de Kubernetes (ej. un `ConfigMap`) como parte de un workflow?

A) `container`
B) `script`
C) **`resource`**
D) `suspend`

---

## 🎯 **Pregunta 20** (Argo CD - ApplicationSets)
¿Qué problema principal resuelven los `ApplicationSets`?

A) La gestión de secretos en Argo CD.
B) La falta de una interfaz de usuario para crear aplicaciones.
C) **La gestión de un gran número de aplicaciones de Argo CD, especialmente en escenarios multi-clúster o multi-entorno.**
D) La lentitud en la sincronización de aplicaciones grandes.

---

## 🎯 **Pregunta 21** (Argo Rollouts - Análisis)
¿Qué sucede si un `AnalysisRun` falla durante un despliegue de Argo Rollouts?

A) El `Rollout` se pausa indefinidamente, esperando intervención manual.
B) **El `Rollout` se aborta y revierte automáticamente (rollback) a la versión estable anterior.**
C) Se envía una notificación, pero el `Rollout` continúa su promoción.
D) Se crea un nuevo `AnalysisRun` y se reintenta el análisis.

---

## 🎯 **Pregunta 22** (Argo Events - Dependencias)
En un `Sensor`, ¿qué define la sección `dependencies`?

A) Las librerías externas que necesita el `Sensor` para funcionar.
B) **La lista de eventos de diferentes `EventSources` que deben ocurrir para que el `trigger` se active.**
C) Otros `Sensors` de los que depende.
D) Los `WorkflowTemplates` que puede ejecutar.

---

## 🎯 **Pregunta 23** (Argo CD - Sincronización)
¿Qué hace la opción `prune` en una `syncPolicy`?

A) Elimina los logs de sincronización antiguos.
B) **Elimina los recursos del clúster que ya no existen en el estado deseado en Git.**
C) Poda las ramas de Git que ya han sido fusionadas.
D) Ignora ciertos recursos durante la sincronización.

---

## 🎯 **Pregunta 24** (Argo Workflows - Paralelismo)
¿Qué estructura se utiliza en un `template` de tipo `steps` para ejecutar un conjunto de tareas en paralelo?

A) `parallel: true`
B) **Una lista de listas (un array de arrays), donde cada elemento de la lista interna se ejecuta en paralelo.**
C) La palabra clave `dag` dentro de `steps`.
D) Todas las tareas en `steps` se ejecutan en paralelo por defecto.

---

## 🎯 **Pregunta 25** (Argo CD - Patrones)
¿Qué es el patrón "App of Apps" en Argo CD?

A) Una característica que permite anidar `ApplicationSets`.
B) **Un patrón donde una `Application` raíz de Argo CD gestiona un conjunto de otras `Applications` hijas.**
C) Una forma de desplegar Argo CD en alta disponibilidad.
D) Un tipo de `AppProject` que agrupa otras aplicaciones.

---

## 🎯 **Pregunta 26** (Argo Rollouts - Estrategias)
¿Qué funcionalidad de Argo Rollouts permite realizar pruebas A/B de forma efectiva?

A) La estrategia `Blue-Green` con `autoPromotionEnabled: false`.
B) **La estrategia `Canary` junto con la manipulación de tráfico de un Ingress o Service Mesh.**
C) El uso de `AnalysisTemplates` con métricas de negocio.
D) La funcionalidad de `pause` en los `steps`.

---

## 🎯 **Pregunta 27** (Argo CD - Comandos)
¿Qué comando de Argo CD muestra las diferencias (drift) entre el estado definido en Git y el estado actual en el clúster?

A) `argocd app status <app-name>`
B) `argocd app compare <app-name>`
C) **`argocd app diff <app-name>`**
D) `argocd app get <app-name> --show-diff`

---

## 🎯 **Pregunta 28** (Argo Workflows - Artefactos)
¿Dónde se configura el `Artifact Repository` por defecto para todos los workflows de un namespace?

A) En un `Secret` llamado `argo-artifact-repository`.
B) **En el `ConfigMap` del `workflow-controller` (ej. `workflow-controller-configmap`).**
C) En la especificación de cada `Workflow`.
D) En las anotaciones del `ServiceAccount` que ejecuta los workflows.

---

## 🎯 **Pregunta 29** (Argo CD - ApplicationSets)
¿Cuál de los siguientes `generators` de `ApplicationSet` es ideal para un escenario multi-clúster donde se quiere desplegar una aplicación en todos los clústeres registrados en Argo CD?

A) List Generator
B) Git Generator
C) **Cluster Generator**
D) Matrix Generator

---

## 🎯 **Pregunta 30** (Argo Events - EventBus)
¿Cuál es la función principal del `EventBus` en Argo Events?

A) Almacenar los `Sensors` y `EventSources`.
B) **Actuar como un bus de mensajería para transportar eventos desde las `EventSources` hasta los `Sensors`.**
C) Ejecutar los `triggers` definidos en los `Sensors`.
D) Validar la sintaxis de los eventos recibidos.

---

## 🎯 **Pregunta 31** (Argo Rollouts - Análisis)
En un `AnalysisTemplate`, ¿qué es una `successCondition`?

A) El número de veces que un análisis debe ser exitoso para promover el `Rollout`.
B) Un comando que se ejecuta si el análisis tiene éxito.
C) **Una expresión lógica que define cuándo el resultado de una métrica se considera exitoso.**
D) El estado final del `Rollout` si el análisis tiene éxito.

---

## 🎯 **Pregunta 32** (Argo Workflows - Timeouts)
¿Cómo se implementan correctamente los timeouts a nivel de un `step` individual en un workflow?

A) Usando `spec.ttlStrategy` en el manifiesto del `Workflow`.
B) **Usando el campo `activeDeadlineSeconds` dentro de la definición del `step`.**
C) Configurando un `timeout` en el `ConfigMap` del controlador.
D) Añadiendo un comando `timeout` al inicio del `script` o `container`.

---

## 🎯 **Pregunta 33** (Argo CD - Sincronización)
¿Qué significa la opción `Replace=true` en los `syncOptions` de una `Application`?

A) Reemplaza el repositorio Git de la aplicación por uno nuevo.
B) **Hace que Argo CD elimine y recree un recurso en lugar de aplicar un parche, útil para campos inmutables.**
C) Reemplaza la estrategia de sincronización de manual a automática.
D) Fuerza una sincronización incluso si la aplicación está `Synced`.

---

## 🎯 **Pregunta 34** (Argo Rollouts - Experimentos)
¿Qué es un `Experiment` en Argo Rollouts?

A) Un sinónimo para la estrategia `Canary`.
B) **Un recurso para ejecutar pruebas A/B/n de tiempo limitado con múltiples variantes de una aplicación, sin afectar el `Rollout` principal.**
C) Una forma de experimentar con diferentes proveedores de métricas.
D) Un análisis que se ejecuta en un entorno de pruebas.

---

## 🎯 **Pregunta 35** (Argo CD - RBAC)
Un usuario se queja de que no puede ver una aplicación en la UI de Argo CD, aunque sabe que existe. ¿Cuál es la causa más probable?

A) El `Repository Server` está caído.
B) La aplicación está en estado `OutOfSync`.
C) **El usuario no tiene los permisos RBAC adecuados o la aplicación no está dentro de un `AppProject` al que el usuario tenga acceso.**
D) El clúster de destino no está respondiendo.

---

## 🏁 Fin del Examen

### 📊 Distribución de Preguntas:
- **Argo CD**: 14 preguntas (40%)
- **Argo Workflows**: 10 preguntas (29%)
- **Argo Events**: 5 preguntas (14%)
- **Argo Rollouts**: 6 preguntas (17%)

### 🎯 Puntaje:
**27/35 preguntas correctas = 77% (APROBADO)**