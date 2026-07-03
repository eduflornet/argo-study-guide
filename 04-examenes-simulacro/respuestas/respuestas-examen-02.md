# ✅ Respuestas - Examen Simulacro 2

> **Instrucciones**: Compara tus respuestas con las correctas y revisa las explicaciones detalladas.

## 🎯 Respuestas Correctas

| # | Respuesta Correcta | Tu Respuesta | ✓/✗ |
|---|---|---|---|
| 1 | B | | |
| 2 | C | | |
| 3 | C | | |
| 4 | C | | |
| 5 | B | | |
| 6 | C | | |
| 7 | B | | |
| 8 | C | | |
| 9 | B | | |
| 10 | B | | |
| 11 | B | | |
| 12 | C | | |
| 13 | C | | |
| 14 | C | | |
| 15 | B | | |
| 16 | B | | |
| 17 | B | | |
| 18 | C | | |
| 19 | C | | |
| 20 | C | | |
| 21 | B | | |
| 22 | B | | |
| 23 | B | | |
| 24 | B | | |
| 25 | B | | |
| 26 | B | | |
| 27 | C | | |
| 28 | B | | |
| 29 | C | | |
| 30 | B | | |
| 31 | C | | |
| 32 | B | | |
| 33 | B | | |
| 34 | B | | |
| 35 | C | | |

---

## 📖 Explicaciones Detalladas

### **Pregunta 1 - `argocd app wait`**
**Respuesta Correcta: B)** Espera a que una aplicación alcance un estado `Synced` y `Healthy`.
**Explicación**: Este comando es crucial en pipelines de CI/CD para pausar un script hasta que el despliegue gestionado por Argo CD se haya completado y estabilizado correctamente en el clúster.

### **Pregunta 2 - `WorkflowTemplate`**
**Respuesta Correcta: C)** Definir una plantilla de workflow reutilizable que puede ser invocada por otros workflows.
**Explicación**: Los `WorkflowTemplates` son la base para la modularidad y la reutilización en Argo Workflows, permitiendo definir una lógica común (un DAG o una secuencia de `steps`) que puede ser llamada desde múltiples `Workflows` o `CronWorkflows`.

### **Pregunta 3 - `Rollout` vs `Deployment`**
**Respuesta Correcta: C)** `Rollout` soporta estrategias avanzadas como Canary y Blue-Green, mientras que `Deployment` no.
**Explicación**: El `Deployment` nativo de Kubernetes solo ofrece estrategias `Recreate` y `RollingUpdate`. Argo Rollouts extiende esta funcionalidad con su CRD `Rollout` para permitir despliegues progresivos y controlados.

### **Pregunta 4 - `EventSource`**
**Respuesta Correcta: C)** EventSource
**Explicación**: La `EventSource` es el componente de Argo Events que actúa como "oyente". Se configura para conectarse a sistemas externos (ej. un endpoint de webhook, un bucket de S3, una cola de NATS) y transformar los eventos de esos sistemas en "CloudEvents" para el `EventBus`.

### **Pregunta 5 - `sync waves`**
**Respuesta Correcta: B)** Controlar el orden de aplicación de los recursos durante una sincronización.
**Explicación**: Las `sync waves` (olas de sincronización) son anotaciones que permiten gestionar dependencias. Por ejemplo, se puede asegurar que un `Namespace` (wave -10) y un `CRD` (wave -5) se creen antes que los recursos que dependen de ellos (wave 0).

### **Pregunta 6 - Dependencias en un `DAG`**
**Respuesta Correcta: C)** Usando `dependencies: [A, B]` en la definición de la tarea `C`.
**Explicación**: La sintaxis `dependencies` es la forma explícita en un `DAG` para definir el grafo de ejecución. Una tarea no comenzará hasta que todas las tareas listadas en sus `dependencies` se hayan completado con éxito.

### **Pregunta 7 - `AppProject`**
**Respuesta Correcta: B)** Proporcionar límites de seguridad y gobernanza, restringiendo repositorios, clústeres y recursos.
**Explicación**: Los `AppProjects` son una característica de seguridad fundamental en Argo CD. Permiten a los administradores aislar equipos y proyectos, controlando a qué repositorios pueden acceder, en qué clústeres/namespaces pueden desplegar y qué tipos de recursos pueden crear.

### **Pregunta 8 - `AnalysisTemplate`**
**Respuesta Correcta: C)** Una plantilla reutilizable que define las métricas y condiciones para evaluar el éxito de una nueva versión.
**Explicación**: El `AnalysisTemplate` desacopla la lógica de análisis del `Rollout`. Define qué métricas consultar (ej. tasa de error de Prometheus), cómo consultarlas y qué umbral (`successCondition`) determina si la nueva versión es estable.

### **Pregunta 9 - Artefactos en Workflows**
**Respuesta Correcta: B)** Usando `outputs.artifacts` en el paso de origen y `inputs.artifacts` en el paso de destino.
**Explicación**: Este es el mecanismo nativo de Argo Workflows para pasar ficheros o directorios entre pasos. Los artefactos se guardan en un repositorio de artefactos (como S3) y se ponen a disposición del siguiente paso.

### **Pregunta 10 - Kustomize Overlays**
**Respuesta Correcta: B)** Apuntando el `path` de la `Application` al directorio del `overlay` correspondiente (ej. `overlays/production`).
**Explicación**: Argo CD detecta automáticamente el fichero `kustomization.yaml` en el `path` especificado. Al apuntar a un directorio de `overlay`, Argo CD ejecutará `kustomize build` en ese directorio, aplicando los parches específicos del entorno sobre la base común.

### **Pregunta 11 - `Sensor`**
**Respuesta Correcta: B)** Sensor
**Explicación**: El `Sensor` es el "cerebro" de Argo Events. Define un conjunto de dependencias de eventos y, una vez que se cumplen, ejecuta uno o más `triggers`, como crear un recurso de Kubernetes, iniciar un Argo Workflow o hacer una llamada HTTP.

### **Pregunta 12 - `withItems` vs `withParam`**
**Respuesta Correcta: C)** `withItems` itera sobre una lista estática, mientras que `withParam` itera sobre una lista JSON generada dinámicamente por un paso anterior.
**Explicación**: `withItems` es para bucles sobre un conjunto de valores conocidos de antemano. `withParam` es mucho más potente, ya que permite crear pipelines dinámicos donde un paso genera una lista de elementos a procesar y el siguiente itera sobre ellos.

### **Pregunta 13 - `selfHeal`**
**Respuesta Correcta: C)** Que cualquier cambio manual en el clúster que cause una desviación (drift) será revertido automáticamente al estado de Git.
**Explicación**: `selfHeal` refuerza el principio de GitOps de que Git es la única fuente de verdad. Si un operador aplica un cambio manual con `kubectl`, Argo CD lo detectará y lo sobrescribirá con la configuración de Git, previniendo la "deriva de configuración".

### **Pregunta 14 - Promover un `Rollout`**
**Respuesta Correcta: C)** `kubectl argo rollouts promote <rollout-name>`
**Explicación**: Cuando un `Rollout` está en estado de pausa (ya sea por una pausa definida en los `steps` o por `autoPromotionEnabled: false`), el comando `promote` es la acción manual necesaria para indicarle que continúe con el siguiente paso de la estrategia.

### **Pregunta 15 - `CronWorkflow`**
**Respuesta Correcta: B)** Un recurso que permite programar la ejecución periódica de un workflow usando sintaxis de `cron`.
**Explicación**: `CronWorkflow` es el equivalente de Argo Workflows a un `CronJob` de Kubernetes. Es ideal para tareas recurrentes como ETLs nocturnos, generación de informes o trabajos de limpieza.

### **Pregunta 16 - Secretos en Helm**
**Respuesta Correcta: B)** Integrándose con herramientas como Sealed Secrets, Vault o SOPS para desencriptar los secretos durante la sincronización.
**Explicación**: La mejor práctica es almacenar secretos en Git de forma encriptada. Argo CD puede configurarse con plugins (como `argocd-vault-plugin` o `sops`) que desencriptan estos secretos de forma segura en el `Repository Server` antes de generar los manifiestos finales.

### **Pregunta 17 - `activeService` en Blue/Green**
**Respuesta Correcta: B)** Es el servicio que siempre apunta a la versión estable que recibe el 100% del tráfico de producción.
**Explicación**: En una estrategia Blue-Green, el `activeService` es el punto de entrada estable para el tráfico. Durante una promoción, Argo Rollouts modifica este servicio para que apunte a la nueva versión (green) en lugar de la antigua (blue).

### **Pregunta 18 - Rol del `Repository Server`**
**Respuesta Correcta: C)** Clonar repositorios Git, generar manifiestos (usando Helm/Kustomize) y mantener una caché.
**Explicación**: El `Repository Server` es el componente que interactúa con Git y las herramientas de templating. El `Application Controller` le pide los manifiestos finales y es este último quien los aplica al clúster.

### **Pregunta 19 - `resource` template**
**Respuesta Correcta: C)** `resource`
**Explicación**: El `template` de tipo `resource` está diseñado específicamente para gestionar el ciclo de vida de recursos de Kubernetes dentro de un workflow. Permite acciones como `create`, `apply`, `patch` y `delete`.

### **Pregunta 20 - `ApplicationSets`**
**Respuesta Correcta: C)** La gestión de un gran número de aplicaciones de Argo CD, especialmente en escenarios multi-clúster o multi-entorno.
**Explicación**: `ApplicationSet` actúa como una "fábrica de aplicaciones". Usando `generators`, puede crear y gestionar automáticamente cientos de `Applications` de Argo CD a partir de una única plantilla, eliminando la necesidad de mantener manifiestos repetitivos.

### **Pregunta 21 - Falla de `AnalysisRun`**
**Respuesta Correcta: B)** El `Rollout` se aborta y revierte automáticamente (rollback) a la versión estable anterior.
**Explicación**: Este es el núcleo del despliegue seguro con Argo Rollouts. Un análisis fallido es una señal de que la nueva versión es inestable, por lo que la automatización toma la decisión segura de revertir inmediatamente para minimizar el impacto.

### **Pregunta 22 - Dependencias en `Sensor`**
**Respuesta Correcta: B)** La lista de eventos de diferentes `EventSources` que deben ocurrir para que el `trigger` se active.
**Explicación**: Las dependencias permiten crear lógica compleja, como "cuando se reciba un webhook Y un fichero aparezca en S3, entonces ejecuta este workflow". El `trigger` no se dispara hasta que todas las dependencias se cumplan.

### **Pregunta 23 - `prune`**
**Respuesta Correcta: B)** Elimina los recursos del clúster que ya no existen en el estado deseado en Git.
**Explicación**: `prune` ayuda a mantener el clúster limpio. Si eliminas un `Service` del repositorio Git, la opción `prune` se asegurará de que ese `Service` también se elimine del clúster durante la siguiente sincronización.

### **Pregunta 24 - Paralelismo en `steps`**
**Respuesta Correcta: B)** Una lista de listas (un array de arrays), donde cada elemento de la lista interna se ejecuta en paralelo.
**Explicación**: La estructura `steps: [[A, B], [C]]` significa: "Ejecuta A y B en paralelo. Cuando ambos terminen, ejecuta C".

### **Pregunta 25 - Patrón "App of Apps"**
**Respuesta Correcta: B)** Un patrón donde una `Application` raíz de Argo CD gestiona un conjunto de otras `Applications` hijas.
**Explicación**: Es un patrón de composición muy potente. Permite tener un repositorio Git que define el estado de todo un entorno (listando las `Applications` que deben desplegarse). La `Application` raíz apunta a este repositorio, y Argo CD se encarga de crear/actualizar/eliminar las aplicaciones hijas.

### **Pregunta 26 - Pruebas A/B**
**Respuesta Correcta: B)** La estrategia `Canary` junto con la manipulación de tráfico de un Ingress o Service Mesh.
**Explicación**: La estrategia Canary permite tener dos versiones corriendo simultáneamente con una división de tráfico. Al integrarse con un Ingress o Service Mesh, se puede configurar el enrutamiento basado en cabeceras HTTP, cookies o peso, lo que es la base para las pruebas A/B.

### **Pregunta 27 - `argocd app diff`**
**Respuesta Correcta: C)** `argocd app diff <app-name>`
**Explicación**: El comando `diff` es fundamental para el troubleshooting y la visibilidad. Muestra una comparación línea por línea entre los manifiestos generados a partir de Git y los que están actualmente aplicados en el clúster, resaltando cualquier "drift".

### **Pregunta 28 - `Artifact Repository` por defecto**
**Respuesta Correcta: B)** En el `ConfigMap` del `workflow-controller` (ej. `workflow-controller-configmap`).
**Explicación**: Configurar el repositorio de artefactos a nivel del controlador es la forma recomendada de establecer una configuración global para todos los workflows, evitando tener que repetirla en cada manifiesto individual.

### **Pregunta 29 - `Cluster Generator`**
**Respuesta Correcta: C)** Cluster Generator
**Explicación**: El `Cluster Generator` itera sobre los clústeres registrados en Argo CD (a través de sus `Secrets`) y genera una `Application` por cada uno, usando el nombre y la URL del servidor del clúster como parámetros en la plantilla.

### **Pregunta 30 - `EventBus`**
**Respuesta Correcta: B)** Actuar como un bus de mensajería para transportar eventos desde las `EventSources` hasta los `Sensors`.
**Explicación**: El `EventBus` es el intermediario que desacopla a los productores de eventos (`EventSources`) de los consumidores (`Sensors`). Proporciona un mecanismo de transporte duradero y fiable para los eventos.

### **Pregunta 31 - `successCondition`**
**Respuesta Correcta: C)** Una expresión lógica que define cuándo el resultado de una métrica se considera exitoso.
**Explicación**: Es una condición que se evalúa contra el resultado de la consulta de la métrica. Por ejemplo, `result[0] < 0.01` significa "el análisis es exitoso si el primer valor devuelto por la consulta es menor que 0.01".

### **Pregunta 32 - Timeouts en `step`**
**Respuesta Correcta: B)** Usando el campo `activeDeadlineSeconds` dentro de la definición del `step`.
**Explicación**: `activeDeadlineSeconds` es un campo nativo de la especificación de Pods de Kubernetes que Argo Workflows expone a nivel de `step`. Si el pod asociado al `step` no finaliza dentro de este tiempo, se considera fallido.

### **Pregunta 33 - `Replace=true`**
**Respuesta Correcta: B)** Hace que Argo CD elimine y recree un recurso en lugar de aplicar un parche, útil para campos inmutables.
**Explicación**: Por defecto, Argo CD usa `kubectl apply`. Sin embargo, algunos campos de los recursos de Kubernetes son inmutables (no se pueden cambiar después de la creación). `Replace=true` le dice a Argo CD que use `kubectl replace --force` (delete & create) para manejar estos casos.

### **Pregunta 34 - `Experiment`**
**Respuesta Correcta: B)** Un recurso para ejecutar pruebas A/B/n de tiempo limitado con múltiples variantes de una aplicación, sin afectar el `Rollout` principal.
**Explicación**: Los `Experiments` son ideales para probar varias versiones candidatas (`templates`) simultáneamente. Cada variante tiene su propio `ReplicaSet` y puede ser analizada de forma independiente. Al final, se puede elegir una para promoverla o simplemente descartar el experimento.

### **Pregunta 35 - RBAC en Argo CD**
**Respuesta Correcta: C)** El usuario no tiene los permisos RBAC adecuados o la aplicación no está dentro de un `AppProject` al que el usuario tenga acceso.
**Explicación**: Este es el problema de permisos más común en Argo CD. La visibilidad de las aplicaciones está estrictamente controlada por la configuración RBAC (`argocd-rbac-cm`) y las políticas definidas en los `AppProjects`.