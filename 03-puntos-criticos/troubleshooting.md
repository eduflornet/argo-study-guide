# ⚡ Troubleshooting - Puntos Críticos CAPA

> **🎯 CRÍTICO**: Esta sección cubre los problemas más comunes del examen y escenarios de troubleshooting

## 🚨 Problemas Más Frecuentes en el Examen

### **1. Application OutOfSync Issues**

#### **Síntomas:**
- Application muestra status `OutOfSync`
- Cambios en Git no se reflejan en cluster
- `Last Sync` time no actualiza

#### **Diagnóstico:**
```bash
# Verificar estado de la aplicación
argocd app get myapp
kubectl describe application myapp -n argocd

# Ver diferencias exactas
argocd app diff myapp

# Verificar logs del controller
kubectl logs -f -n argocd deployment/argocd-application-controller
```

#### **Causas Comunes:**
```yaml
Causes:
  ❌ Sync policy manual (no auto-sync)
  ❌ Git repository inaccessible  
  ❌ Syntax errors in YAML
  ❌ Resource quotas exceeded
  ❌ RBAC permissions insufficient
  ❌ Helm/Kustomize generate errors
```

#### **Soluciones:**
```bash
# 1. Manual sync para forzar actualización
argocd app sync myapp

# 2. Enable auto-sync
argocd app set myapp --sync-policy automated

# 3. Verificar conectividad a Git
argocd repo get https://github.com/user/repo

# 4. Check configuration
kubectl get secret argocd-repo-creds-* -n argocd
```

---

### **2. Application Degraded/Unhealthy**

#### **Síntomas:**
- Health status: `Degraded` o `Progressing`
- Resources no ready
- Pods en estado CrashLoopBackOff

#### **Diagnóstico:**
```bash
# Ver health status detallado
argocd app get myapp --show-params

# Verificar recursos específicos
kubectl get pods,svc,deploy -l app.kubernetes.io/instance=myapp

# Logs de recursos problemáticos
kubectl logs -l app.kubernetes.io/instance=myapp
kubectl describe pod <pod-name>
```

#### **Health Check Personalizado:**
```yaml
# Custom health check en ArgoCD ConfigMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
data:
  resource.customizations.health.argoproj.io_Rollout: |
    hs = {}
    if obj.status ~= nil then
      if obj.status.replicas ~= nil and obj.status.updatedReplicas ~= nil then
        if obj.status.replicas == obj.status.updatedReplicas then
          hs.status = "Healthy"
          hs.message = "Rollout is healthy"
        else
          hs.status = "Progressing"  
          hs.message = "Rolling out new version"
        end
      end
    end
    return hs
```

---

### **3. Sync Hooks Failures**

#### **Hook Types y Order:**
```yaml
Hooks Execution Order:
  1. PreSync     (wave: -5)
  2. Sync        (wave: 0-N) 
  3. PostSync    (wave: +5)
  4. SyncFail    (only on failure)
```

#### **Hook Configuration:**
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: database-migration
  annotations:
    argocd.argoproj.io/hook: PreSync
    argocd.argoproj.io/hook-delete-policy: BeforeHookCreation
    argocd.argoproj.io/sync-wave: "1"
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
      - name: migrate
        image: migrate/migrate
        command: ["migrate", "-path", "/migrations", "-database", "$DB_URL", "up"]
```

#### **Common Hook Issues:**
```yaml
Issues:
  ❌ Hook job never completes (no RestartPolicy: Never)
  ❌ Hook fails but sync continues (no failure policy)
  ❌ Wrong sync wave order
  ❌ Hook delete policy not set
  ❌ Insufficient RBAC for hook resources
```

#### **Debugging Hooks:**
```bash
# Ver hook execution history
argocd app get myapp --show-operation

# Verificar jobs/pods de hooks
kubectl get jobs -l app.kubernetes.io/instance=myapp
kubectl logs job/database-migration
```

---

### **4. Repository Connection Issues**

#### **Síntomas:**
- "repository not accessible" errors
- Git fetch failures
- Certificate/auth errors

#### **Repository Types:**
```yaml
Git Repository Access:
  - HTTPS with username/password
  - HTTPS with token
  - SSH with private key
  - GitHub App authentication
  - Git LFS repositories
```

#### **Diagnostic Commands:**
```bash
# Test repository connection
argocd repo get https://github.com/user/repo

# List all repositories  
argocd repo list

# Verify credentials
kubectl get secrets -n argocd | grep repo-creds
kubectl get secret argocd-repo-creds-XXX -o yaml
```

#### **Common Solutions:**
```yaml
# SSH Key Repository
apiVersion: v1
kind: Secret
metadata:
  name: private-repo-creds
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: git@github.com:private/repo.git
  sshPrivateKey: |
    -----BEGIN OPENSSH PRIVATE KEY-----
    b3BlbnNzaC1rZXktdjE...
    -----END OPENSSH PRIVATE KEY-----
```

```yaml
# HTTPS Token Repository  
apiVersion: v1
kind: Secret
metadata:
  name: https-repo-creds
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/private/repo.git
  password: ghp_xxxxxxxxxxxxxxxxxxxx
  username: not-used
```

---

### **5. RBAC and Permission Issues**

#### **Síntomas:**
- "permission denied" en sync operations
- User no puede ver applications
- Service account errors

#### **RBAC Hierarchy:**
```mermaid
graph TD
    A[User/ServiceAccount] --> B[ArgoCD Role]
    B --> C[AppProject Policy] 
    C --> D[Kubernetes RBAC]
    D --> E[Resource Access]
    
    style A fill:#e1f5fe
    style B fill:#f3e5f5  
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

#### **AppProject RBAC:**
```yaml
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: myproject
spec:
  # Restrict source repositories
  sourceRepos:
  - 'https://github.com/myorg/*'
  
  # Restrict destination clusters/namespaces
  destinations:
  - namespace: 'myproject-*'
    server: https://kubernetes.default.svc
    
  # Define roles for this project
  roles:
  - name: developer
    description: Developer role
    policies:
    - p, proj:myproject:developer, applications, get, myproject/*, allow
    - p, proj:myproject:developer, applications, sync, myproject/*, allow
    groups:
    - myorg:developers
```

#### **User Roles:**
```yaml
# ArgoCD ConfigMap for user roles
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
data:
  policy.csv: |
    p, role:admin, applications, *, */*, allow
    p, role:developer, applications, get, */*, allow
    p, role:developer, applications, sync, */dev-*, allow
    g, myorg:admins, role:admin
    g, myorg:developers, role:developer
```

#### **Service Account Issues:**
```bash
# Verificar service account permissions
kubectl auth can-i create applications --as=system:serviceaccount:argocd:argocd-application-controller -n myproject

# Ver ArgoCD SA permissions  
kubectl describe clusterrolebinding argocd-application-controller
kubectl describe clusterrole argocd-application-controller
```

---

### **6. Helm Integration Issues**

#### **Common Helm Problems:**
```yaml
Issues:
  ❌ Helm values not applied correctly
  ❌ Chart dependencies not resolved  
  ❌ Template rendering failures
  ❌ Helm hooks conflicting with ArgoCD hooks
  ❌ Chart version constraints
```

#### **Helm Application Configuration:**
```yaml
apiVersion: argoproj.io/v1alpha1  
kind: Application
metadata:
  name: helm-app
spec:
  source:
    repoURL: https://charts.example.com
    chart: mychart
    targetRevision: "1.2.3"
    helm:
      # Values file override
      valueFiles:
      - values-production.yaml
      
      # Inline values
      values: |
        replica: 3
        image:
          tag: v1.2.3
          
      # Parameters override
      parameters:
      - name: service.type
        value: LoadBalancer
```

#### **Debugging Helm Issues:**
```bash
# Ver rendered manifests
argocd app manifests helm-app

# Verificar valores aplicados
argocd app get helm-app --show-params

# Test chart rendering localmente  
helm template mychart ./chart --values values-prod.yaml
```

---

### **7. Kustomize Integration Issues**

#### **Common Kustomize Problems:**
```yaml
Issues:
  ❌ Invalid kustomization.yaml
  ❌ Resource not found in overlay
  ❌ Name/namespace collisions  
  ❌ Strategic merge patch errors
  ❌ Missing base resources
```

#### **Kustomize Application:**
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application  
metadata:
  name: kustomize-app
spec:
  source:
    repoURL: https://github.com/user/app-config
    targetRevision: HEAD
    path: overlays/production
    kustomize:
      # Override images
      images:
      - myapp:v1.2.3
      
      # Override replicas
      replicas:
      - name: myapp
        count: 5
        
      # Apply patches
      patches:
      - target:
          kind: Deployment
          name: myapp
        patch: |
          - op: replace
            path: /spec/template/spec/containers/0/resources/requests/memory
            value: 512Mi
```

#### **Debugging Kustomize:**
```bash
# Ver resultado de kustomize build
argocd app manifests kustomize-app

# Test build localmente
kustomize build overlays/production

# Verificar estructura
kubectl kustomize --dry-run=client overlays/production
```

---

## ⚡ Quick Reference - Comandos de Troubleshooting

### **Diagnóstico Rápido:**
```bash
# Application status overview
argocd app list
argocd app get <app-name>

# Ver diferencias pendientes
argocd app diff <app-name>

# Force sync
argocd app sync <app-name>

# Ver historial de sync
argocd app history <app-name>

# Rollback a revisión anterior
argocd app rollback <app-name> <revision-id>
```

### **Logs y Debugging:**
```bash
# Controller logs
kubectl logs -n argocd deployment/argocd-application-controller

# API Server logs  
kubectl logs -n argocd deployment/argocd-server

# Repository Server logs
kubectl logs -n argocd deployment/argocd-repo-server

# Application resources
kubectl get all -l app.kubernetes.io/instance=<app-name>
```

### **Configuration Check:**
```bash
# Verify ArgoCD installation
kubectl get pods -n argocd
kubectl get cm argocd-cm -n argocd -o yaml

# Check repositories  
argocd repo list
kubectl get secrets -n argocd -l argocd.argoproj.io/secret-type=repository

# User & RBAC
argocd account list
kubectl get cm argocd-rbac-cm -n argocd -o yaml
```

---

## 🎯 Puntos Críticos para el Examen

### **Memorizar Estados de Application:**
- **OutOfSync**: Git ≠ Cluster (requiere sync)
- **Synced**: Git = Cluster (estado correcto)  
- **Healthy**: Recursos funcionando correctamente
- **Degraded**: Algunos recursos con problemas
- **Progressing**: Sync/update en progreso

### **Orden de Troubleshooting:**
1. **Check Application Status** → `argocd app get`
2. **Verify Repository Access** → `argocd repo get`  
3. **Check Sync Differences** → `argocd app diff`
4. **Review Controller Logs** → `kubectl logs`
5. **Verify RBAC** → `kubectl auth can-i`

### **Common Exam Scenarios:**
- Application stuck in OutOfSync → Check auto-sync policy
- Deployment failing → Check resource quotas & RBAC
- Helm chart not updating → Check values & chart version  
- Repository connection failing → Check credentials & network
- Hook failing → Check job completion & delete policy

---

## ❓ Escenarios de Práctica

1. **Una Application está OutOfSync pero auto-sync está habilitado. ¿Cuáles son las posibles causas?**

2. **Un Helm chart no se actualiza después de cambiar values.yaml. ¿Cómo diagnosticarías el problema?**

3. **Los PreSync hooks fallan constantemente. ¿Qué verificarías?**

4. **Un usuario no puede ver las Applications asignadas. ¿Cómo verificarías RBAC?**

5. **After deployment, pods are in CrashLoopBackOff but ArgoCD shows Synced. ¿Cómo resolverías el health check?**