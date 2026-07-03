# ✅ Respuestas - Examen Simulacro 3

> **Instrucciones**: Compara tus respuestas con las correctas y revisa las explicaciones detalladas.

## 🎯 Respuestas Correctas

| # | Respuesta Correcta | Tu Respuesta | ✓/✗ |
|---|---|---|---|
| 1 | C | | |
| 2 | B | | |
| 3 | C | | |
| 4 | B | | |
| 5 | C | | |
| 6 | C | | |
| 7 | C | | |
| 8 | B | | |
| 9 | A, B, C, D | | |
| 10 | C | | |
| 11 | B | | |
| 12 | B | | |
| 13 | C | | |
| 14 | C | | |
| 15 | B | | |
| 16 | C | | |
| 17 | C | | |
| 18 | B | | |
| 19 | B | | |
| 20 | B | | |
| 21 | C | | |
| 22 | A, B, C, D | | |
| 23 | C | | |
| 24 | B | | |
| 25 | B | | |
| 26 | B | | |
| 27 | B | | |
| 28 | B | | |
| 29 | B | | |
| 30 | B | | |
| 31 | B | | |
| 32 | D | | |
| 33 | C | | |
| 34 | C | | |
| 35 | C | | |

---

## 📖 Explicaciones Detalladas

### **Pregunta 1 - Principios de GitOps**
**Respuesta Correcta: C)** Los cambios deben ser aplicados manualmente por un operador para mayor seguridad.
**Explicación**: Esto contradice el tercer principio de GitOps, que aboga por la **aplicación automática** de los cambios para reducir el error humano y aumentar la velocidad.

### **Pregunta 2 - `Refresh` vs `Sync`**
**Respuesta Correcta: B)** `Refresh` actualiza el estado de comparación con Git, mientras que `Sync` aplica los cambios al clúster.
**Explicación**: `Refresh` es una operación de "lectura" que comprueba si hay nuevos commits en Git y recalcula el estado `OutOfSync`. `Sync` es una operación de "escritura" que reconcilia el estado del clúster con el de Git.

### **Pregunta 3 - `Artifact Repository`**
**Respuesta Correcta: C)** Un almacenamiento externo (ej. S3) para guardar los artefactos de salida, ya que no se almacenan en el clúster.
**Explicación**: Los pods de los workflows son efímeros. Para que los datos (ficheros, logs, etc.) persistan y se puedan compartir entre pasos, Argo Workflows necesita un repositorio de artefactos configurado.

### **Pregunta 4 - `autoPromotionEnabled: false`**
**Respuesta Correcta: B)** Pausa el `Rollout` después de que la versión `green` esté lista y el `previewService` esté disponible, requiriendo una promoción manual.
**Explicación**: Esta opción proporciona un punto de control crucial en la estrategia Blue-Green, permitiendo realizar pruebas de aceptación o validaciones manuales en la versión `green` a través del `previewService` antes de hacer el cambio de tráfico.

### **Pregunta 5 - `Sensor`**
**Respuesta Correcta: C)** Sensor
**Explicación**: El `Sensor` es el componente que consume eventos del `EventBus`, evalúa si se cumplen las dependencias definidas y, en caso afirmativo, ejecuta los `triggers` configurados.

### **Pregunta 6 - `Application Controller`**
**Respuesta Correcta: C)** Application Controller
**Explicación**: El `Application Controller` es el corazón de Argo CD. Monitorea las `Applications`, compara el estado real con el deseado (obtenido del `Repository Server`) y ejecuta las acciones (`kubectl apply`, etc.) para sincronizar el clúster.

### **Pregunta 7 - `withSequence`**
**Respuesta Correcta: C)** Para ejecutar un `template` un número determinado de veces, pasando un índice numérico en cada iteración.
**Explicación**: `withSequence` es un generador de bucles que itera sobre un rango numérico. Es útil para tareas que necesitan ser ejecutadas un número fijo de veces o que requieren un índice.

### **Pregunta 8 - `resource hook`**
**Respuesta Correcta: B)** Un recurso de Kubernetes (como un `Job`) que se ejecuta en diferentes fases del ciclo de sincronización (ej. `PreSync`, `PostSync`).
**Explicación**: Los hooks son una forma de extender el ciclo de vida de la sincronización para realizar tareas auxiliares, como migraciones de bases de datos (`PreSync`) o pruebas de humo (`PostSync`).

### **Pregunta 9 - Proveedores de Métricas**
**Respuesta Correcta: A, B, C, D)** Todas son correctas.
**Explicación**: Argo Rollouts tiene un sistema de proveedores de métricas extensible. Soporta nativamente Prometheus, Datadog, y otros, pero también permite ejecutar un `Job` de Kubernetes para obtener una métrica de forma personalizada o usar Kayenta para análisis estadístico avanzado.

### **Pregunta 10 - Ventaja de `DAG`**
**Respuesta Correcta: C)** Los `DAGs` permiten definir dependencias complejas y no lineales, optimizando el paralelismo.
**Explicación**: Mientras que `steps` define una secuencia lineal (con paralelismo limitado a cada paso), un `DAG` permite modelar flujos de trabajo donde las tareas pueden tener múltiples dependencias, permitiendo una ejecución más eficiente y paralela.

### **Pregunta 11 - `OutOfSync`**
**Respuesta Correcta: B)** El estado real de los recursos en el clúster no coincide con el estado deseado definido en Git.
**Explicación**: Este estado, también conocido como "drift" (desviación), es el indicador principal de que se necesita una sincronización. Puede ser causado por un nuevo commit en Git o por un cambio manual en el clúster.

### **Pregunta 12 - `EventBus` de Producción**
**Respuesta Correcta: B)** NATS Streaming
**Explicación**: Aunque Argo Events soporta una implementación `In-Memory` para pruebas, no es persistente. NATS Streaming (o ahora JetStream) es la opción recomendada para producción porque ofrece durabilidad y garantías de entrega "at-least-once".

### **Pregunta 13 - `Git Generator`**
**Respuesta Correcta: C)** Git Generator (con `directories`)
**Explicación**: El `Git Generator` puede descubrir ficheros o directorios en un repositorio Git y usar esa información para generar `Applications`. El modo `directories` es perfecto para el patrón donde cada subdirectorio representa una aplicación a desplegar.

### **Pregunta 14 - `argo submit`**
**Respuesta Correcta: C)** `argo submit`
**Explicación**: `argo submit` es el comando principal de la CLI de Argo Workflows para crear y ejecutar una nueva instancia de un `Workflow` a partir de un fichero YAML.

### **Pregunta 15 - Manipulación de Tráfico en Rollouts**
**Respuesta Correcta: B)** Integrándose y manipulando recursos de un Ingress Controller (como NGINX) o un Service Mesh (como Istio).
**Explicación**: Argo Rollouts delega la manipulación del tráfico a la infraestructura de red existente. El `Rollout Controller` actualiza los recursos del Ingress (ej. `Ingress` de NGINX) o del Service Mesh (ej. `VirtualService` de Istio) para dividir el tráfico según los `steps` de la estrategia.

### **Pregunta 16 - Helm `valueFiles`**
**Respuesta Correcta: C)** `helm.valueFiles: ["values.yaml"]`
**Explicación**: La sintaxis correcta en la especificación de la `Application` es `valueFiles` (plural, pero sin la 's' extra al final). `valuesFiles` (con 's' extra) es un error común.

### **Pregunta 17 - `concurrencyPolicy: Forbid`**
**Respuesta Correcta: C)** Previene que se inicie una nueva ejecución del workflow si la anterior todavía está en curso.
**Explicación**: Esta política es crucial para trabajos programados que no deben ejecutarse en paralelo. Si un ETL diario tarda más de 24 horas, `Forbid` evitará que se lance una segunda instancia, previniendo conflictos.

### **Pregunta 18 - `ignoreDifferences`**
**Respuesta Correcta: B)** Para que Argo CD no marque la aplicación como `OutOfSync` debido a cambios en campos específicos (ej. `replicas` gestionado por un HPA).
**Explicación**: Es una herramienta para gestionar "drift" esperado. Si otro controlador (como un HorizontalPodAutoscaler) modifica un campo, `ignoreDifferences` le dice a Argo CD que ese cambio es aceptable y no requiere una sincronización.

### **Pregunta 19 - `failureLimit`**
**Respuesta Correcta: B)** El número de mediciones consecutivas fallidas de una métrica que causarán que el `AnalysisRun` falle.
**Explicación**: `failureLimit` ayuda a evitar falsos negativos debidos a fluctuaciones momentáneas en las métricas. El análisis no fallará en la primera medición fallida, sino que requerirá `N` fallos consecutivos.

### **Pregunta 20 - `script` template**
**Respuesta Correcta: B)** `script`
**Explicación**: El `template` de tipo `script` es una abstracción sobre el `template` `container`. Permite inyectar un bloque de código (`source`) en un contenedor con el intérprete adecuado (Python, Bash, etc.), ejecutándolo sin necesidad de construir una imagen personalizada.

### **Pregunta 21 - `argocd cluster`**
**Respuesta Correcta: C)** `argocd cluster ...`
**Explicación**: El subcomando `argocd cluster` se utiliza para listar, añadir y eliminar clústeres remotos que Argo CD puede gestionar como destinos de despliegue.

### **Pregunta 22 - Triggers de `Sensor`**
**Respuesta Correcta: A, B, C, D)** Todas son correctas.
**Explicación**: Los `triggers` de Argo Events son muy versátiles. Pueden interactuar con el ecosistema de Argo (Workflows, Rollouts), gestionar recursos de Kubernetes, o integrarse con sistemas externos a través de peticiones HTTP o colas de mensajes.

### **Pregunta 23 - Revertir un `Rollout`**
**Respuesta Correcta: C)** `kubectl argo rollouts abort <rollout-name>`
**Explicación**: El comando `abort` detiene inmediatamente el proceso de despliegue y revierte todo el tráfico a la `ReplicaSet` estable anterior. Es la acción de "parada de emergencia".

### **Pregunta 24 - TTL en Workflows**
**Respuesta Correcta: B)** Configurando `spec.ttlStrategy.secondsAfterCompletion: 600` en el manifiesto del `Workflow`.
**Explicación**: La `ttlStrategy` es el mecanismo nativo de Argo Workflows para la recolección de basura (garbage collection). Permite limpiar automáticamente los recursos de los workflows finalizados (tanto `Succeeded` como `Failed`) después de un tiempo.

### **Pregunta 25 - Kustomize `images`**
**Respuesta Correcta: B)** En el fichero `kustomization.yaml` del `overlay` correspondiente, usando el campo `images`.
**Explicación**: La forma idiomática de usar Kustomize es definir las personalizaciones (como el tag de la imagen) en el `kustomization.yaml` del `overlay`. Argo CD simplemente ejecuta `kustomize build` sobre ese directorio.

### **Pregunta 26 - `pause` con `duration`**
**Respuesta Correcta: B)** Pausa el `Rollout` durante 5 minutos y luego lo promueve automáticamente al siguiente paso si no hay errores.
**Explicación**: Una pausa con `duration` es una pausa temporal. Es útil para dar tiempo a que las métricas se estabilicen o para una breve revisión manual, pero el proceso continúa automáticamente si no se interviene. Una pausa sin duración (`pause: {}`) es indefinida.

### **Pregunta 27 - `Sync Windows`**
**Respuesta Correcta: B)** Ventanas de tiempo configurables que permiten o deniegan la sincronización de aplicaciones, útiles para implementar "deployment freezes".
**Explicación**: Son una característica de gobernanza clave. Permiten, por ejemplo, denegar todas las sincronizaciones en un `AppProject` durante el horario comercial para evitar interrupciones, y permitirlas solo por la noche.

### **Pregunta 28 - `spec.entrypoint`**
**Respuesta Correcta: B)** Especificar cuál de los `templates` definidos en el manifiesto es el punto de inicio del workflow.
**Explicación**: Un manifiesto de `Workflow` puede contener muchos `templates`. El `entrypoint` le dice a Argo Workflows cuál de ellos debe ejecutar primero para iniciar el flujo de trabajo.

### **Pregunta 29 - Réplica única del `Application Controller`**
**Respuesta Correcta: B)** Porque es un componente con estado (stateful) que mantiene bloqueos sobre las aplicaciones que gestiona, y múltiples réplicas podrían causar conflictos de reconciliación.
**Explicación**: Para evitar que dos controladores intenten sincronizar la misma aplicación al mismo tiempo, el `Application Controller` utiliza un sistema de sharding y liderazgo. Aunque se pueden ejecutar múltiples réplicas para tolerancia a fallos, solo una estará activa para un conjunto de aplicaciones en un momento dado.

### **Pregunta 30 - `previewService`**
**Respuesta Correcta: B)** Es un servicio interno que apunta a la nueva versión (green) y puede ser usado para pruebas de aceptación antes de la promoción.
**Explicación**: Mientras el `activeService` maneja el tráfico de producción, el `previewService` proporciona un punto de acceso estable a la versión `green`, permitiendo que los equipos de QA o sistemas automatizados de prueba la validen en aislamiento.

### **Pregunta 31 - Salida de Parámetros**
**Respuesta Correcta: B)** Escribiendo el valor en un fichero y declarándolo como un `outputs.parameters` con una `valueFrom.path`.
**Explicación**: Esta es la forma estándar de extraer un valor de un fichero para usarlo como parámetro. El `workflow-controller` lee el contenido del fichero en la ruta especificada y lo hace disponible como un parámetro de salida.

### **Pregunta 32 - Autenticación no soportada**
**Respuesta Correcta: D)** Credenciales de usuario/contraseña sin codificar en la URL.
**Explicación**: Por razones de seguridad, Argo CD no permite el uso de contraseñas en texto plano. Se deben usar tokens de acceso personal o autenticación de aplicaciones (como GitHub Apps o claves SSH).

### **Pregunta 33 - Lógica OR en `Sensor`**
**Respuesta Correcta: C)** Definiendo dos `dependencies` separadas en la lista, ya que la lógica por defecto entre ellas es OR.
**Explicación**: La lógica dentro de un `Sensor` es: AND dentro de un `dependencyGroup`, y OR entre `dependencyGroups`. Si se listan dependencias directamente, cada una actúa como su propio grupo, por lo que la lógica entre ellas es OR.

### **Pregunta 34 - Acceso a Resultados de Prometheus**
**Respuesta Correcta: C)** `result[0]`
**Explicación**: El resultado de una consulta de Prometheus (y de la mayoría de los proveedores) se devuelve como un array de valores. `result[0]` se utiliza para acceder al valor de la primera serie temporal devuelta, que es la convención más común.

### **Pregunta 35 - Secretos en Workflows**
**Respuesta Correcta: C)** Creando un `Secret` de Kubernetes y referenciándolo a través de `valueFrom.secretKeyRef` en las variables de entorno.
**Explicación**: Esta es la práctica recomendada y nativa de Kubernetes para manejar secretos. Desacopla el secreto del manifiesto del `Workflow`, permitiendo que sea gestionado de forma segura por los administradores del clúster.