# 📝 Examen Simulacro 1 - CAPA

> **⏰ Tiempo**: 60 minutos | **Preguntas**: 30 | **Puntaje**: 75% para aprobar

## 📋 Instrucciones
- Marca la **mejor respuesta** para cada pregunta
- Algunas preguntas pueden tener múltiples respuestas correctas
- Lee cada pregunta cuidadosamente
- Revisa tus respuestas antes de enviar

---

## 🎯 **Pregunta 1** (Argo CD - Arquitectura)
¿Cuál componente de Argo CD es responsable de clonar repositorios Git y generar manifests de Kubernetes?

A) API Server  
B) **Repository Server**  
C) Application Controller  
D) Redis  

---

## 🎯 **Pregunta 2** (GitOps - Principios)
Según los principios fundamentales de GitOps, ¿cuáles de las siguientes afirmaciones son correctas? (Selecciona todas las correctas)

A) **El estado deseado debe expresarse declarativamente**  
B) Los cambios pueden aplicarse manualmente cuando sea necesario  
C) **Git es la única fuente de verdad**  
D) **Los software agents deben monitorear continuamente el estado**  

---

## 🎯 **Pregunta 3** (Argo CD - Applications)
¿Cuál comando correcto crea una Application en Argo CD con sync automático?

A) `argocd create app myapp --repo http://github.com/user/repo --sync auto`  
B) `kubectl create application myapp --git-repo http://github.com/user/repo`  
**C) `argocd app create myapp --repo http://github.com/user/repo --path ./k8s --dest-server https://kubernetes.default.svc --sync-policy automated`**  
D) `argocd app new myapp --source http://github.com/user/repo --auto-sync`  

---

## 🎯 **Pregunta 4** (Argo CD - Sync Policies)
Una Application muestra el estado "OutOfSync" pero tiene `syncPolicy.automated` configurado. ¿Cuáles podrían ser las causas? (Selecciona todas las correctas)

A) **Errores de sintaxis en los manifests YAML**  
B) El repository server está funcionando correctamente  
C) **Permisos RBAC insuficientes**  
D) **El repositorio Git no es accesible**  

---

## 🎯 **Pregunta 5** (Argo Workflows - Básico)
¿Cuál es el comando correcto para ejecutar un workflow usando Argo Workflows?

A) `argo run workflow.yaml`  
B) `argo execute workflow.yaml`  
**C) `argo submit workflow.yaml`**  
D) `argo create workflow.yaml`  

---

## 🎯 **Pregunta 6** (Argo CD - Health Status)
¿Qué significa el health status "Degraded" en una Application de Argo CD?

A) La aplicación está completamente inaccesible  
**B) Algunos recursos tienen problemas pero otros funcionan**
C) La aplicación nunca ha sido desplegada  
D) La sincronización está en progreso  

---

## 🎯 **Pregunta 7** (Argo CD - Hooks)
¿En qué orden se ejecutan los sync hooks en Argo CD?

A) PostSync → Sync → PreSync  
B) Sync → PreSync → PostSync  
C) **PreSync → Sync → PostSync**  
D) El orden no importa  

---

## 🎯 **Pregunta 8** (Argo Rollouts - Estrategias)
¿Cuáles son las estrategias de deployment soportadas por Argo Rollouts? (Selecciona todas las correctas)

A) **Canary**  
B) **Blue-Green**  
C) Rolling Update  
D) Recreate  

---

## 🎯 **Pregunta 9** (Argo CD - Multi-cluster)
Para gestionar múltiples clusters con Argo CD, ¿qué se debe configurar en la Application?

A) Múltiples source repositories  
B) **Destination server diferente a kubernetes.default.svc**  
C) Múltiples sync policies  
D) Diferentes namespaces ArgoCD  

---

## 🎯 **Pregunta 10** (Argo Events - Componentes)
¿Cuáles son los dos componentes principales de Argo Events? (Selecciona dos)

**A) Event Sources**  
B) Event Processors  
**C) Sensors**  
D) Event Handlers  

---

## 🎯 **Pregunta 11** (Argo CD - Troubleshooting)
Un usuario reporta que no puede ver las Applications asignadas en la UI. ¿Cuál sería el primer paso de troubleshooting?

A) Verificar el estado del cluster  
B) **Revisar configuración RBAC y AppProject policies**  
C) Restart ArgoCD components  
D) Verificar la conectividad del repositorio  

---

## 🎯 **Pregunta 12** (Argo Workflows - Terminología)
En Argo Workflows, ¿qué es un "Template"?

A) Un workflow completo  
**B) Una definición reutilizable de pasos o tareas** 
C) Un parámetro de configuración  
D) Un resultado de ejecución  

---

## 🎯 **Pregunta 13** (Argo CD - Projects)
¿Cuál es el propósito principal de los AppProjects en Argo CD?

A) Organizar Applications por tipo  
**B) Proveer boundaries de seguridad y governance**
C) Mejorar performance de sync  
D) Facilitar backup y restore  

---

## 🎯 **Pregunta 14** (GitOps - vs CI/CD)
¿Qué ventaja principal tiene el modelo "Pull" de GitOps comparado con el modelo "Push" tradicional de CI/CD?

A) Mayor velocidad de deployment  
B) **Menor exposición de credenciales**  
C) Soporte para más lenguajes de programación  
D) Mejor integración con herramientas de testing  

---

## 🎯 **Pregunta 15** (Argo CD - Sync Waves)
¿Para qué se utilizan las sync waves en Argo CD?

A) Para sincronizar múltiples clusters  
B) **Para controlar el orden de aplicación de recursos**  
C) Para configurar retry policies  
D) Para definir health checks customizados  

---

## 🎯 **Pregunta 16** (Argo Rollouts - Comandos)
¿Cuál comando se usa para promover un rollout canary al 100% del tráfico?

A) `kubectl argo rollouts complete <rollout-name>`  
B) `kubectl argo rollouts finish <rollout-name>`  
**C) `kubectl argo rollouts promote <rollout-name> --full`**  
D) `kubectl argo rollouts approve <rollout-name>`  

---

## 🎯 **Pregunta 17** (Argo CD - Repository Connection)
¿Cuál tipo de autenticación NO es soportado por Argo CD para conectarse a repositorios Git?

A) SSH private key  
B) HTTPS with token  
C) **OAuth2 implicit flow**  
D) GitHub App authentication  

---

## 🎯 **Pregunta 18** (Argo Workflows - Estados)
¿Cuáles son estados válidos para un Workflow en Argo Workflows? (Selecciona todas las correctas)

A) **Running**  
B) **Succeeded**  
C) **Failed**  
D) **Pending**  

---

## 🎯 **Pregunta 19** (Argo CD - Self-Heal)
Cuando está habilitada la opción "self-heal" en una Application, ¿qué sucede si alguien hace cambios manuales al cluster?

A) Los cambios se conservan permanentemente  
B) **ArgoCD revierte los cambios automáticamente**  
C) Se envía una notificación pero no se toma acción  
D) Se marca la aplicación como degraded  

---

## 🎯 **Pregunta 20** (Argo Events - EventBus)
En Argo Events, ¿cuál es la función del EventBus?

A) Ejecutar workflows automáticamente  
B) **Transportar eventos entre Event Sources y Sensors**  
C) Almacenar configuración de eventos  
D) Autenticar event sources  

---

## 🎯 **Pregunta 21** (Argo CD - Helm Integration)
¿Cuál configuración es correcta para usar Helm values files en una Application de Argo CD?

A)  
   ```yaml
   helm:
     values: values-prod.yaml
   ```
B)  
   ```yaml
   helm:
     valuesFiles: 
     - values-prod.yaml
   ```
**C)** 
   ```yaml
   helm:
     valueFiles:
     - values-prod.yaml
   ```
D)  
   ```yaml
   helm:
     files:
     - values-prod.yaml
   ```

---

## 🎯 **Pregunta 22** (Argo Rollouts - Analysis)
¿Qué componente de Argo Rollouts se encarga de evaluar métricas durante un deployment?

A) Rollout Controller  
B) **Analysis Template**  
C) Experiment Controller  
D) Traffic Router  

---

## 🎯 **Pregunta 23** (Argo CD - Resource Management)
¿Qué sucede cuando se habilita "auto-prune" en una Application?

A) Los recursos viejos se actualizan automáticamente  
B) **Los recursos que ya no están en Git se eliminan del cluster**  
C) Los recursos fallidos se reinician automáticamente  
D) Los recursos se respaldan antes de cada sync  

---

## 🎯 **Pregunta 24** (Argo Workflows - Parallelism)
Para ejecutar múltiples tareas en paralelo en un Workflow, ¿qué estructura se debe usar?

A) sequence  
B) **steps con múltiples elementos en un array**  
C) pipeline  
D) concurrent  

---

## 🎯 **Pregunta 25** (Argo CD - High Availability)
En una configuración de Alta Disponibilidad de Argo CD, ¿cuántas replicas debe tener el Application Controller?

A) 3 replicas para tolerancia a fallos  
B) 2 replicas para active-passive  
C) **1 replica (es un componente stateful)**  
D) 5 replicas para mejor performance  

---

## 🎯 **Pregunta 26** (Argo Events - Triggers)
¿Cuál es un trigger válido para un Sensor en Argo Events?

A) **Argo Workflows**  
B) Jenkins Jobs  
C) GitHub Actions  
D) Azure DevOps Pipelines  

---

## 🎯 **Pregunta 27** (Argo CD - Kustomize)
¿Cuál configuración permite override de imágenes con Kustomize en Argo CD?

A)  
   ```yaml
   kustomize:
     imageOverrides:
     - myapp:v1.2.3
   ```
**B)** 
   ```yaml
   kustomize:
     images:
     - myapp:v1.2.3
   ```
C)  
   ```yaml
   kustomize:
     replaceImages:
     - myapp:v1.2.3
   ```
D)  
   ```yaml
   kustomize:
     newImages:
     - myapp:v1.2.3
   ```

---

## 🎯 **Pregunta 28** (Argo Rollouts - Traffic Management)
¿Cuáles Ingress Controllers son soportados nativamente por Argo Rollouts para traffic splitting? (Selecciona todas las correctas)

A) **NGINX**  
B) **ALB**  
C) **Istio**  
D) **Traefik**  

---

## 🎯 **Pregunta 29** (Argo CD - Refresh vs Sync)
¿Cuál es la diferencia entre "Refresh" y "Sync" en Argo CD?

A) Refresh aplica cambios, Sync los revierte  
B) **Refresh actualiza el estado de comparación, Sync aplica cambios**
C) Refresh es automático, Sync es manual  
D) No hay diferencia, son sinónimos  

---

## 🎯 **Pregunta 30** (Argo Workflows - Secrets)
¿Cuál es la forma recomendada de manejar secrets en Argo Workflows?

A) Hardcodear los secrets en el YAML  
B) **Usar Kubernetes secrets y referenciarlos**
C) Pasarlos como parámetros en línea de comandos  
D) Almacenarlos en ConfigMaps  

---

## 🎯 **Pregunta 31** (Argo Workflows - Loops)
¿Qué se utiliza en Argo Workflows para iterar sobre una lista de elementos generada dinámicamente por un paso anterior?

A) `withItems`  
B) `withSequence`
**C) `withParam`**  
D) `loop`  

---

## 🎯 **Pregunta 32** (Argo Workflows - Scheduling)
¿Qué recurso se utiliza para programar la ejecución periódica de un workflow, similar a un CronJob de Kubernetes?

A) `ScheduledWorkflow`  
**B) `CronWorkflow`**
C) `WorkflowTrigger`  
D) `WorkflowTimer`  

---

## 🎯 **Pregunta 33** (Argo Events - EventBus)
¿Qué implementación de EventBus es la más recomendada para entornos de producción por su persistencia y durabilidad?

A) In-Memory  
B) **NATS Streaming**
C) Kafka  
D) Redis Streams  

---

## 🎯 **Pregunta 34** (Argo Rollouts - Analysis)
Durante un análisis de Argo Rollouts, ¿qué sucede si la `successCondition` de una métrica no se cumple?

A) El rollout se pausa indefinidamente  
B) **El rollout se revierte (rollback) automáticamente**
C) Se envía una notificación y el rollout continúa  
D) Se incrementa el peso del canary para obtener más datos  

---

## 🎯 **Pregunta 35** (Argo Workflows - Templates)
¿Cuál es el propósito de un `WorkflowTemplate`?

A) Definir un único paso de un workflow  
B) Almacenar los artefactos de salida de un workflow
C) **Crear una definición de workflow reutilizable que puede ser invocada por otros workflows**  
D) Programar la ejecución de workflows  

---

## 🏁 Fin del Examen

### 📊 Distribución de Preguntas:
- **Argo CD**: 18 preguntas (51%)
- **Argo Workflows**: 9 preguntas (26%)  
- **Argo Events**: 4 preguntas (11%)
- **Argo Rollouts**: 4 preguntas (11%)

### ⏱️ Gestión del Tiempo:
- **Menos de 2 minutos por pregunta** en promedio
- **Marca las difíciles** y regresa después  
- **Revisa las respuestas** en los últimos 10 minutos

### 🎯 Puntaje:
**27/35 preguntas correctas = 77% (APROBADO)**

---

## 📝 Notas para Revisar:

Después de completar el examen, revisa las siguientes áreas si tuviste dificultades:

- **Applications y Projects** → [03-applications-projects.md](../01-teoria/02-argo-cd/03-applications-projects.md)
- **Sync Policies** → [04-sync-policies.md](../01-teoria/02-argo-cd/04-sync-policies.md)  
- **Troubleshooting** → [troubleshooting.md](../03-puntos-criticos/troubleshooting.md)
- **Comandos** → [comandos-esenciales.md](../03-puntos-criticos/comandos-esenciales.md)

¡Buena suerte! 🍀