# 💻 Comandos Esenciales para CAPA

> **⚡ CRÍTICO**: Estos comandos son fundamentales para el examen y trabajo diario con Argo

## 🎯 Comandos Más Preguntados en el Examen

### **🚀 Argo CD CLI Commands**

#### **Application Management**
```bash
# CREAR APPLICATION (MUY IMPORTANTE)
argocd app create myapp \
  --repo https://github.com/user/repo \
  --path ./k8s \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace production \
  --sync-policy automated \
  --auto-prune \
  --self-heal

# Ver estado de aplicación
argocd app get myapp
argocd app list
argocd app list -o wide

# SYNC OPERATIONS (CRÍTICO)
argocd app sync myapp
argocd app sync myapp --dry-run
argocd app sync myapp --prune
argocd app sync myapp --force

# Ver diferencias pendientes
argocd app diff myapp
argocd app diff myapp --local ./manifests

# Esperar a que la app esté saludable (CRÍTICO para CI/CD)
argocd app wait myapp
argocd app wait myapp --health

# CONFIGURAR APPLICATION
argocd app set myapp --sync-policy automated
argocd app set myapp --auto-prune
argocd app set myapp --self-heal
argocd app set myapp --sync-option CreateNamespace=true

# Historial y rollbacks
argocd app history myapp
argocd app rollback myapp 123
```

#### **Repository Management**
```bash
# LISTAR REPOSITORIOS
argocd repo list
argocd repo get https://github.com/user/repo

# AGREGAR REPOSITORIO (IMPORTANTE)
argocd repo add https://github.com/user/repo
argocd repo add https://github.com/user/repo --ssh-private-key-path ~/.ssh/id_rsa
argocd repo add https://github.com/user/repo --username myuser --password mytoken

# Helm repositories  
argocd repo add https://charts.helm.sh/stable --type helm --name stable
```

#### **Cluster Management**
```bash
# LISTAR CLUSTERS
argocd cluster list
argocd cluster get https://kubernetes.default.svc

# AGREGAR CLUSTER EXTERNO
argocd cluster add context-name
argocd cluster add context-name --namespace argocd
```

#### **Project Management**
```bash
# Ver projects
argocd proj list
argocd proj get default
argocd proj get myproject

# Crear project
argocd proj create myproject \
  --dest https://kubernetes.default.svc,* \
  --src https://github.com/myorg/*
```

---

### **🔧 kubectl Commands para Argo CD**

#### **Applications**
```bash
# LISTAR APPLICATIONS (MUY ÚTIL)
kubectl get applications -n argocd
kubectl get apps -n argocd
kubectl get applications -A

# VER DETALLES DE APPLICATION
kubectl describe application myapp -n argocd
kubectl get application myapp -n argocd -o yaml
kubectl get application myapp -n argocd -o jsonpath='{.status.sync.status}'

# Editar application
kubectl edit application myapp -n argocd

# Eliminar application
kubectl delete application myapp -n argocd
```

#### **AppProjects**
```bash
# Listar projects
kubectl get appprojects -n argocd
kubectl get appproj -n argocd

# Ver detalles
kubectl describe appproject myproject -n argocd
kubectl get appproject myproject -n argocd -o yaml
```

#### **Argo CD Components**
```bash
# VERIFICAR PODS DE ARGO CD
kubectl get pods -n argocd
kubectl get pods -n argocd -l app.kubernetes.io/part-of=argocd

# Logs de componentes (IMPORTANTE PARA TROUBLESHOOTING)
kubectl logs -n argocd deployment/argocd-application-controller
kubectl logs -n argocd deployment/argocd-server
kubectl logs -n argocd deployment/argocd-repo-server
kubectl logs -n argocd deployment/argocd-redis

# Estado de servicios
kubectl get svc -n argocd
kubectl get ingress -n argocd
```

#### **Configuration**
```bash
# Ver configuración principal
kubectl get cm argocd-cm -n argocd -o yaml

# RBAC configuration
kubectl get cm argocd-rbac-cm -n argocd -o yaml

# Repository credentials
kubectl get secrets -n argocd -l argocd.argoproj.io/secret-type=repository

# Ver secret de repo específico
kubectl get secret argocd-repo-creds-xxxxx -n argocd -o yaml
```

---

### **🚀 Argo Workflows Commands**

#### **Workflow Management**
```bash
# EJECUTAR WORKFLOW (IMPORTANTE)
argo submit workflow.yaml
argo submit workflow.yaml --parameter-file params.yaml
argo submit workflow.yaml -p param1=value1 -p param2=value2

# LISTAR WORKFLOWS
argo list
argo list -n default
argo list --running
argo list --completed

# VER ESTADO DE WORKFLOW
argo get workflowname
argo get workflowname -o yaml
argo logs workflowname
argo logs workflowname -f

# GESTIÓN DE WORKFLOWS
argo delete workflowname
argo stop workflowname
argo retry workflowname
argo resubmit workflowname
```

#### **Templates y WorkflowTemplates**
```bash
# WorkflowTemplates
argo template list
argo template get template-name
argo template create template.yaml

# ClusterWorkflowTemplates  
argo cluster-template list
argo cluster-template get template-name
```

#### **kubectl para Workflows**
```bash
# Ver workflows como recursos K8s
kubectl get workflows
kubectl get wf
kubectl get workflows -o wide

# WorkflowTemplates
kubectl get workflowtemplates
kubectl get wftmpl

# Cronworkflows
kubectl get cronworkflows
kubectl get cronwf
```

---

### **⚡ Argo Events Commands**

#### **Event Sources**
```bash
# Ver event sources
kubectl get eventsources
kubectl get es

# Detalles de event source
kubectl describe eventsource webhook-source
kubectl get eventsource webhook-source -o yaml
```

#### **Sensors**  
```bash
# Ver sensors
kubectl get sensors
kubectl get sensor

# Detalles de sensor
kubectl describe sensor webhook-sensor
kubectl get sensor webhook-sensor -o yaml
```

#### **Logs y Debugging**
```bash
# Event source logs
kubectl logs -l eventsource-name=webhook-source

# Sensor logs  
kubectl logs -l sensor-name=webhook-sensor

# EventBus logs
kubectl logs -l app=eventbus
```

---

### **🎯 Argo Rollouts Commands**

#### **Rollout Management**
```bash
# VER ROLLOUTS
kubectl argo rollouts list rollouts
kubectl argo rollouts get rollout rollout-name

# PROMOTE ROLLOUT (MUY IMPORTANTE)
kubectl argo rollouts promote rollout-name
kubectl argo rollouts promote rollout-name --full

# ABORT ROLLOUT
kubectl argo rollouts abort rollout-name

# RESTART ROLLOUT
kubectl argo rollouts restart rollout-name

# ROLLBACK
kubectl argo rollouts undo rollout-name
kubectl argo rollouts undo rollout-name --to-revision=3
```

#### **Analysis Templates**
```bash
# Ver analysis templates
kubectl get analysistemplates
kubectl get at

# Cluster analysis templates
kubectl get clusteranalysistemplates
kubectl get cat
```

#### **Experiments**
```bash
# Ver experiments
kubectl get experiments
kubectl get exp

# Detalles de experiment
kubectl describe experiment experiment-name
```

---

## 🎯 Patrones de Comandos Importantes

### **Debugging Pattern**
```bash
# 1. Ver el recurso
kubectl get <resource> <name> -n <namespace>

# 2. Describir para eventos
kubectl describe <resource> <name> -n <namespace>

# 3. Ver YAML completo
kubectl get <resource> <name> -n <namespace> -o yaml

# 4. Ver logs si aplica
kubectl logs <pod> -n <namespace>

# 5. Usar ArgoCD CLI si es app
argocd app get <app-name>
```

### **Sync Troubleshooting Pattern**
```bash
# 1. Ver estado actual
argocd app get myapp

# 2. Ver diferencias
argocd app diff myapp

# 3. Forzar sync si necesario
argocd app sync myapp --force

# 4. Ver logs del controller
kubectl logs -n argocd deployment/argocd-application-controller
```

---

## 🔥 Comandos Críticos para Memorizar

### **🎯 Top 10 Must-Know Commands:**

1. **`argocd app create`** - Crear applications
2. **`argocd app sync`** - Sincronizar cambios
3. **`argocd app get`** - Ver estado de app
4. **`argocd app diff`** - Ver diferencias pendientes
5. **`kubectl get applications -n argocd`** - Listar apps
6. **`kubectl logs -n argocd deployment/argocd-application-controller`** - Troubleshooting
7. **`argo submit`** - Ejecutar workflows
8. **`argo list`** - Ver workflows
9. **`kubectl argo rollouts promote`** - Promover rollouts
10. **`kubectl get pods -n argocd`** - Verificar componentes

### **💡 Command Patterns para el Examen:**

#### **Application Lifecycle:**
```bash
# Create → Sync → Monitor → Troubleshoot
argocd app create → argocd app sync → argocd app get → kubectl logs
```

#### **Workflow Lifecycle:**  
```bash
# Submit → Monitor → Debug → Cleanup
argo submit → argo get → argo logs → argo delete
```

#### **Rollout Lifecycle:**
```bash  
# Deploy → Monitor → Promote → Rollback
kubectl apply → kubectl argo rollouts get → kubectl argo rollouts promote → kubectl argo rollouts undo
```

---

## 📝 Command Cheat Sheet

### **Argo CD Essentials**
```bash
# Applications
argocd app list                    # List all apps
argocd app get <app>              # Get app details  
argocd app sync <app>             # Sync application
argocd app diff <app>             # Show differences
argocd app delete <app>           # Delete application

# Configuration
argocd app set <app> --sync-policy automated
argocd app set <app> --auto-prune --self-heal
argocd repo add <url>             # Add repository
argocd cluster add <context>      # Add cluster
```

### **kubectl for Argo CD**
```bash
# Resources
kubectl get applications -n argocd
kubectl get appprojects -n argocd  
kubectl describe app <app> -n argocd

# Components
kubectl get pods -n argocd
kubectl logs deployment/argocd-application-controller -n argocd
```

### **Argo Workflows**
```bash
# Workflows  
argo submit <workflow.yaml>       # Submit workflow
argo list                        # List workflows
argo get <workflow>              # Get workflow details
argo logs <workflow>             # View logs
argo delete <workflow>           # Delete workflow
```

### **Argo Rollouts**
```bash
# Rollouts
kubectl argo rollouts list rollouts
kubectl argo rollouts get rollout <rollout>
kubectl argo rollouts promote <rollout>
kubectl argo rollouts abort <rollout>
kubectl argo rollouts restart <rollout>
```

---

## ❓ Práctica de Comandos

### **Escenarios del Examen:**

1. **Crear una Application que se sincronice automáticamente:**
   ```bash
   argocd app create myapp --repo https://github.com/user/repo --path ./k8s --dest-server https://kubernetes.default.svc --dest-namespace default --sync-policy automated
   ```

2. **Ver por qué una Application está OutOfSync:**
   ```bash  
   argocd app get myapp
   argocd app diff myapp
   ```

3. **Forzar sync de una Application problemática:**
   ```bash
   argocd app sync myapp --force --prune
   ```

4. **Verificar logs cuando algo falla:**
   ```bash
   kubectl logs -n argocd deployment/argocd-application-controller
   ```

5. **Promover un Rollout canary:**
   ```bash
   kubectl argo rollouts promote rollout-name
   ```

### **⚡ Command Memory Tips:**

- **`argocd app`** = Todas las operaciones de Applications
- **`kubectl get applications`** = Ver recursos como K8s objects
- **`argo`** = Workflows operations  
- **`kubectl argo rollouts`** = Rollouts operations
- **`kubectl logs deployment/argocd-<component>`** = Troubleshooting logs