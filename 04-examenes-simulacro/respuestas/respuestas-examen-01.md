# ✅ Respuestas - Examen Simulacro 1

> **Instrucciones**: Compara tus respuestas con las correctas y revisa las explicaciones detalladas

## 🎯 Respuestas Correctas

| # | Respuesta Correcta | Tu Respuesta | ✓/✗ |
|---|---|---|---|
| 1 | B | | |
| 2 | A, C, D | | |
| 3 | C | | |
| 4 | A, C, D | | |
| 5 | C | | |
| 6 | B | | |
| 7 | C | | |
| 8 | A, B | | |
| 9 | B | | |
| 10 | A, C | | |
| 11 | B | | |
| 12 | B | | |
| 13 | B | | |
| 14 | B | | |
| 15 | B | | |
| 16 | C | | |
| 17 | C | | |
| 18 | A, B, C, D | | |
| 19 | B | | |
| 20 | B | | |
| 21 | C | | |
| 22 | B | | |
| 23 | B | | |
| 24 | B | | |
| 25 | C | | |
| 26 | A | | |
| 27 | B | | |
| 28 | A, B, C, D | | |
| 29 | B | | |
| 30 | B | | |
| 31 | C | | |
| 32 | B | | |
| 33 | B | | |
| 34 | B | | |
| 35 | C | | |

---

## 📖 Explicaciones Detalladas

### **Pregunta 1 - (Argo CD - Arquitectura)**
**Respuesta Correcta: B) Repository Server**

**Explicación**: El Repository Server es el componente específico encargado de:
- Clonar repositories Git
- Procesar herramientas como Helm y Kustomize  
- Generar los manifests finales de Kubernetes
- Cachear el contenido de los repositorios

Los otros componentes tienen funciones diferentes:
- **API Server**: UI y API endpoints
- **Application Controller**: Aplica cambios al cluster
- **Redis**: Cache y sesiones

### **Pregunta 2 - (GitOps - Principios)**
**Respuesta Correcta: A, C, D**

**Explicación**: Los 4 principios fundamentales de GitOps son:
1. ✅ **Declarativo**: Estado expresado declarativamente
2. ✅ **Versionado**: Git como única fuente de verdad
3. ✅ **Automático**: Cambios aplicados automáticamente (NO manualmente)
4. ✅ **Monitoreado**: Software agents monitorean continuamente

La opción B es incorrecta porque va contra el principio de aplicación automática.

### **Pregunta 3 - (Argo CD - Applications)**
**Respuesta Correcta: C**

**Explicación**: La sintaxis correcta para `argocd app create` incluye:
- `--repo`: URL del repositorio
- `--path`: Path dentro del repo  
- `--dest-server`: Cluster destino
- `--sync-policy automated`: Para auto-sync

Las otras opciones tienen sintaxis incorrecta o comandos inexistentes.

### **Pregunta 4 - (Argo CD - Sync Policies)**
**Respuesta Correcta: A, C, D**

**Explicación**: Si una app tiene auto-sync pero está OutOfSync, las causas pueden ser:
- ✅ **Errores YAML**: Manifests inválidos impiden el sync
- ✅ **RBAC**: Sin permisos para aplicar recursos
- ✅ **Repo inaccesible**: No puede obtener manifests

Si el repository server funciona bien (opción B), no causaría OutOfSync.

### **Pregunta 5 - (Argo Workflows - Básico)**
**Respuesta Correcta: C) `argo submit`**

**Explicación**: `argo submit` es el comando estándar para ejecutar workflows. Los otros comandos no existen en Argo Workflows CLI.

### **Pregunta 6 - (Argo CD - Health Status)**
**Respuesta Correcta: B) Algunos recursos tienen problemas pero otros funcionan**

**Explicación**: 
- **Healthy**: Todos los recursos OK
- **Degraded**: Algunos recursos con problemas
- **Progressing**: Deployment en progreso
- **Missing**: Recursos no encontrados

El estado "Degraded" indica que los manifiestos se aplicaron, pero uno o más recursos no están saludables (ej. un pod en `CrashLoopBackOff`).

### **Pregunta 7 - (Argo CD - Hooks)**
**Respuesta Correcta: C) PreSync → Sync → PostSync**

**Explicación**: El orden lógico es:
1. **PreSync**: Preparación (ej: backup DB)
2. **Sync**: Aplicación normal de recursos
3. **PostSync**: Finalizacion (ej: notificaciones)

### **Pregunta 8 - (Argo Rollouts - Estrategias)**
**Respuesta Correcta: A, B (Canary, Blue-Green)**

**Explicación**: Argo Rollouts soporta específicamente:
- **Canary**: Deployment gradual con % de tráfico
- **Blue-Green**: Switch completo entre versiones

Rolling Update y Recreate son estrategias nativas de Kubernetes, no de Rollouts.

### **Pregunta 9 - (Argo CD - Multi-cluster)**
**Respuesta Correcta: B) Destination server diferente**

**Explicación**: 
Para que Argo CD gestione múltiples clusters, cada Application debe indicar a qué cluster debe desplegar.
Esto se hace configurando:

**spec.destination.server** → URL del API server del cluster destino (remoto)
**spec.destination.namespace** → namespace dentro de ese cluster

Cuando quieres desplegar a otro cluster distinto del cluster donde corre Argo CD, debes usar un destination server diferente a **https://kubernetes.default.svc**, que es el cluster local.

### **Pregunta 10 - (Argo Events - Componentes)**
**Respuesta Correcta: A, C (Event Sources, Sensors)**

**Explicación**: Los dos componentes principales son:
- **Event Sources**: Generan eventos (webhooks, S3, etc.)
- **Sensors**: Consumen eventos y ejecutan triggers

### **Pregunta 11 - (Argo CD - Troubleshooting)**
**Respuesta Correcta: B) Revisar RBAC y AppProject policies**

**Explicación**: Si un usuario no ve Applications asignadas, típicamente es un problema de:
- Configuración RBAC incorrecta
- AppProject policies restrictivas
- Falta de mapping entre usuario y roles

### **Pregunta 12 - (Argo Workflows - Terminología)**
**Respuesta Correcta: B) Definición reutilizable de pasos**

**Explicación**: Los Templates en Argo Workflows son unidades reutilizables que definen:
- Pasos específicos de ejecución
- Containers o scripts a ejecutar
- Pueden ser reutilizados por múltiples workflows

### **Pregunta 13 - (Argo CD - Projects)**
**Respuesta Correcta: B) Security boundaries y governance**

**Explicación**: Los AppProjects son la herramienta principal de Argo CD para el multi-tenancy y la gobernanza. Permiten restringir qué repositorios, clústeres de destino y tipos de recursos pueden usar un conjunto de aplicaciones.

### **Pregunta 14 - (GitOps - vs CI/CD)**
**Respuesta Correcta: B) Menor exposición de credenciales**

**Explicación**: En el modelo Pull:
 - Solo el agent en el cluster necesita credenciales K8s
 - CI/CD no necesita acceso directo al cluster
 - Reduce superficie de ataque significativamente

### **Pregunta 15 - (Argo CD - Sync Waves)**
**Respuesta Correcta: B) Controlar orden de aplicación**

**Explicación**: Sync waves (`argocd.argoproj.io/sync-wave`) determinan:
- El orden en que se aplican recursos
- Útil para dependencias (ej: namespace antes que pods)
- Wave -5 → Wave 0 → Wave 5

### **Pregunta 16 - (Argo Rollouts - Comandos)**
**Respuesta Correcta: C) `kubectl argo rollouts promote <rollout> --full`**

**Explicación**: 
- `promote` avanza el rollout al siguiente step
- `--full` lo lleva directamente al 100%
- Sin `--full` avanza step by step

### **Pregunta 17 - (Argo CD - Repository Connection)**
**Respuesta Correcta: C) OAuth2 implicit flow**

**Explicación**: Argo CD soporta:
- ✅ SSH private key
- ✅ HTTPS with token  
- ✅ GitHub App auth
- ❌ OAuth2 implicit flow (no soportado)

### **Pregunta 18 - (Argo Workflows - Estados)**
**Respuesta Correcta: A, B, C, D (Todas)**

**Explicación**: Estados válidos de Workflows:
- **Pending**: Esperando ejecución
- **Running**: En ejecución
- **Succeeded**: Completado exitosamente
- **Failed**: Falló durante ejecución

### **Pregunta 19 - (Argo CD - Self-Heal)**
**Respuesta Correcta: B) ArgoCD revierte automáticamente**

**Explicación**: Con self-heal habilitado:
- ArgoCD detecta drift (diferencias)
- Automaticamente aplica el estado deseado de Git
- Revierte cambios manuales no autorizados

### **Pregunta 20 - (Argo Events - EventBus)**
**Respuesta Correcta: B) Transportar eventos entre Event Sources y Sensors**

**Explicación**: EventBus es el middleware que:
- Recibe eventos de Event Sources
- Los entrega a los Sensors correspondientes
- Maneja persistencia y confiabilidad

### **Pregunta 21 - (Argo CD - Helm Integration)**
**Respuesta Correcta: C) `valueFiles:` (no `valuesFiles`)**

**Explicación**: La sintaxis correcta es:
```yaml
helm:
  valueFiles:    # Correcto
  - values-prod.yaml
```

### **Pregunta 22 - Analysis Component**
**Respuesta Correcta: B) Analysis Template**

**Explicación**: Analysis Templates:
- Definen métricas a evaluar
- Conectan con providers (Prometheus, etc.)
- Determinan éxito/fallo de deployments

### **Pregunta 23 - Auto-Prune**
**Respuesta Correcta: B) Recursos no en Git se eliminan**

**Explicación**: Auto-prune:
- Detecta recursos en cluster que no están en Git
- Los elimina automáticamente
- Mantiene cluster sincronizado con Git

### **Pregunta 24 - Parallel Execution**
**Respuesta Correcta: B) Steps con múltiples elementos**

**Explicación**: Para paralelismo en workflows:
```yaml
steps:
- - name: task1    # Ejecutan en paralelo
    template: job1
  - name: task2    # Ejecutan en paralelo  
    template: job2
```

### **Pregunta 25 - (Argo CD - High Availability)**
**Respuesta Correcta: C) 1 replica (stateful component)**

**Explicación**: El Application Controller:
- Es stateful (mantiene estado de reconciliación)

**El Application Controller** de Argo CD **no debe ejecutarse con múltiples réplicas**, incluso en configuraciones de Alta Disponibilidad (HA).
Esto se debe a que:

Es un componente **stateful**.

Mantiene colas internas de operaciones y estado de reconciliación.

No está diseñado para funcionar con múltiples réplicas simultáneas sin sharding explícito.

Ejecutarlo con más de una réplica **puede causar condiciones de carrera, duplicación de operaciones o inconsistencias**.

Por eso, incluso en despliegues HA oficiales, **el Application Controller se mantiene con 1 sola réplica**, a menos que se configure sharding, que es un caso avanzado y no forma parte del despliegue HA estándar.

### **Pregunta 26 - (Argo Events - Triggers)**
**Respuesta Correcta: A) Argo Workflows**

**Explicación**: Triggers válidos para Sensors incluyen:
- ✅ Argo Workflows
- ✅ Kubernetes resources

### **Pregunta 27 - (Argo CD - Kustomize)**
**Respuesta Correcta: B) `images:`**

**Explicación**: Para override de imágenes con Kustomize:
```yaml
kustomize:
  images:        # Correcto
  - myapp:v1.2.3
```

### **Pregunta 28 - Traffic Management**
**Respuesta Correcta: A, B, C, D (Todas)**

**Explicación**: Argo Rollouts soporta nativamente:
- ✅ **NGINX** Ingress Controller
- ✅ **ALB** (AWS Load Balancer)
- ✅ **Istio** Service Mesh
- ✅ **Traefik** Ingress Controller

### **Pregunta 29 - Refresh vs Sync**
**Respuesta Correcta: B) Refresh actualiza comparación, Sync aplica cambios**

**Explicación**:
- **Refresh**: Actualiza la comparación Git vs Cluster
- **Sync**: Aplica los cambios detectados al cluster
- Refresh es informativo, Sync es acción

### **Pregunta 30 - Secrets Management**
**Respuesta Correcta: B) Usar Kubernetes secrets y referenciarlos**

**Explicación**: Best practice para secrets:
```yaml
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

### **Pregunta 31 - (Argo Workflows - Loops)**
**Respuesta Correcta: C) `withParam`**

**Explicación**: Para iterar sobre elementos generados dinámicamente:
- **`withParam`**: Usa la salida JSON de un paso anterior para iterar
- **`withItems`**: Lista estática de elementos definida en el template
- **`withSequence`**: Genera secuencia numérica (ej: 1-10)

Ejemplo `withParam`:
```yaml
steps:
- - name: generate-list
    template: list-generator
- - name: process-item
    template: processor
    withParam: "{{steps.generate-list.outputs.result}}"
```

### **Pregunta 32 - (Argo Workflows - Scheduling)**  
**Respuesta Correcta: B) `CronWorkflow`**

**Explicación**: `CronWorkflow` es el recurso equivalente a CronJob para workflows:
- Programa ejecución basada en expresiones cron
- Genera instancias de Workflow automáticamente
- Soporte para timezone y gestión de concurrencia

Ejemplo:
```yaml
apiVersion: argoproj.io/v1alpha1
kind: CronWorkflow
metadata:
  name: nightly-backup
spec:
  schedule: "0 2 * * *"  # 2 AM diariamente
  workflowSpec:
    entrypoint: backup-task
```

### **Pregunta 33 - (Argo Events - EventBus)**
**Respuesta Correcta: B) NATS Streaming**

**Explicación**: NATS Streaming es la implementación recomendada porque:
- **Persistencia**: Los eventos sobreviven a reinicios
- **Durabilidad**: Garantía de entrega de eventos
- **Escalabilidad**: Maneja alto throughput de eventos
- **Confiabilidad**: Soporte para clustering y HA

Implementaciones disponibles:
- **In-Memory**: Solo para desarrollo (no persistente)
- **NATS Streaming**: Recomendado para producción
- **Redis Streams**: Alternativa válida pero menos común

### **Pregunta 34 - (Argo Rollouts - Analysis)**
**Respuesta Correcta: B) El rollout se revierte automáticamente**

**Explicación**: Durante el análisis de Argo Rollouts:
- Si `successCondition` falla → **Rollback automático**
- Si `failureLimit` se excede → **Rollback automático**  
- El rollout regresa a la versión anterior (stable)
- Previene deployments problemáticos en producción

Ejemplo Analysis Template:
```yaml
successCondition: result[0] >= 0.95  # 95% success rate
failureLimit: 3  # Máximo 3 fallas antes de rollback
```

### **Pregunta 35 - (Argo Workflows - Templates)**
**Respuesta Correcta: C) Definición reutilizable para otros workflows**

**Explicación**: `WorkflowTemplate` permite:
- **Reutilización**: Mismo template en múltiples workflows
- **Versionado**: Templates como recursos K8s versionables
- **Gobernanza**: Control centralizado de lógica de workflows
- **Mantenimiento**: Cambios centralizados que aplican a todos

Diferencias:
- **Template**: Definición dentro de un workflow
- **WorkflowTemplate**: Recurso independiente reutilizable
- **ClusterWorkflowTemplate**: WorkflowTemplate a nivel cluster

---

## 📊 Análisis de Resultados

### **Puntaje Total: ___/30 (____%)**

### **Por Área:**
- **Argo CD (18 preguntas)**: ___/18 (___%)
- **Argo Workflows (6 preguntas)**: ___/6 (___%)  
- **Argo Events (3 preguntas)**: ___/3 (___%)
- **Argo Rollouts (3 preguntas)**: ___/3 (___%)

### **Interpretación:**
- **27-30 (90-100%)**: 🏆 Excelente preparación
- **23-26 (77-87%)**: ✅ Bien preparado, revisar áreas débiles
- **18-22 (60-73%)**: ⚠️ Necesitas más estudio
- **<18 (<60%)**: ❌ Requiere estudio intensivo

### **Áreas a Reforzar:**
✏️ Anota aquí las áreas donde tuviste más errores para enfocar tu estudio.

---

## 📚 Recursos para Áreas Débiles

### **Si fallaste muchas de Argo CD:**
- Revisar: [Arquitectura ArgoCD](../01-teoria/02-argo-cd/01-arquitectura-argocd.md)
- Practicar: [Laboratorios Argo CD](../02-practica/01-laboratorios-argo-cd/)
- Comandos: [Comandos Esenciales](../03-puntos-criticos/comandos-esenciales.md)

### **Si fallaste muchas de Workflows:**
- Revisar: [Teoría Workflows](../01-teoria/03-argo-workflows/)
- Practicar: [Laboratorios Workflows](../02-practica/02-laboratorios-workflows/)

### **Si fallaste muchas de Events/Rollouts:**
- Revisar: [Teoría Events](../01-teoria/04-argo-events/)  
- Revisar: [Teoría Rollouts](../01-teoria/05-argo-rollouts/)

---

¡Continúa estudiando y practica con el siguiente simulacro! 🚀