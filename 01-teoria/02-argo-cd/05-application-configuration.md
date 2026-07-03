# 📱 Application Configuration

## ¿Qué es una Argo CD Application?

Una **Application** es el **custom resource principal** de Argo CD que define **qué deploy**, **dónde deploy** y **cómo mantener sincronizado** una aplicación entre Git repository y Kubernetes cluster.

## 🧩 Anatomía de una Application

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-application           # Application name
  namespace: argocd             # Must be in Argo CD namespace
  finalizers:
  - resources-finalizer.argocd.argoproj.io
spec:
  # 1. What to deploy (Source)
  source:
    repoURL: https://github.com/org/repo
    path: k8s/
    targetRevision: HEAD
    
  # 2. Where to deploy (Destination)  
  destination:
    server: https://kubernetes.default.svc
    namespace: production
    
  # 3. How to deploy (Sync Policy)
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
      
  # 4. Project assignment
  project: default
```

## 🎯 Application Source Configuration

### **Git Repository Sources**

#### **Plain Kubernetes YAML**
```yaml
spec:
  source:
    repoURL: https://github.com/myorg/k8s-manifests
    path: applications/frontend/
    targetRevision: HEAD
    # Argo CD applies all YAML files in the path
```

#### **Multiple Sources (Multi-Source Apps)**
```yaml
spec:
  sources:
  # Application manifests
  - repoURL: https://github.com/myorg/app-config
    path: k8s/
    targetRevision: HEAD
    
  # Helm values from different repo
  - repoURL: https://github.com/myorg/helm-values  
    path: values/production.yaml
    targetRevision: HEAD
    
  # Third source for secrets
  - repoURL: https://github.com/myorg/secrets
    path: sealed-secrets/
    targetRevision: HEAD
```

#### **Path Selection Patterns**
```yaml
source:
  repoURL: https://github.com/myorg/monorepo
  path: services/user-service/k8s    # Specific service path
  # path: services/*/k8s            # Wildcard not supported
  # Use ApplicationSet for multiple paths
```

#### **Branch/Tag/Commit Targeting**
```yaml
source:
  repoURL: https://github.com/myorg/configs
  path: manifests/
  
  # Branch targeting
  targetRevision: main              # Latest commit on main
  targetRevision: develop          # Latest commit on develop
  targetRevision: feature/new-ui   # Latest commit on feature branch
  
  # Tag targeting  
  targetRevision: v1.2.3           # Specific tag
  targetRevision: release-*         # Not supported, use specific tag
  
  # Commit targeting
  targetRevision: 7a2b3c4d5e6f     # Specific commit SHA
  targetRevision: HEAD             # Latest commit on default branch
  targetRevision: HEAD~1           # Previous commit
```

### **Helm Chart Sources**

#### **Helm Chart from Git**
```yaml
source:
  repoURL: https://github.com/myorg/helm-charts
  path: charts/my-app
  targetRevision: HEAD
  helm:
    # Values files (in order of precedence)
    valueFiles:
    - values.yaml                 # Default values
    - values-prod.yaml           # Environment-specific values
    - secrets/values-secret.yaml # Secret values
    
    # Parameter overrides (highest precedence)
    parameters:
    - name: image.repository
      value: myregistry.io/myapp
    - name: image.tag
      value: v1.2.3
    - name: replicaCount
      value: "5"
    - name: service.type
      value: LoadBalancer
      
    # Inline values (YAML)
    values: |
      ingress:
        enabled: true
        hosts:
        - host: myapp.example.com
          paths: ["/"]
        tls:
        - secretName: myapp-tls
          hosts: ["myapp.example.com"]
          
      resources:
        requests:
          memory: 256Mi
          cpu: 200m
        limits:
          memory: 512Mi
          cpu: 500m
```

#### **Helm Chart from OCI Registry**
```yaml
source:
  repoURL: oci://registry.example.com/charts  # OCI registry
  chart: my-application                       # Chart name (not path)
  targetRevision: 1.2.3                      # Chart version
  helm:
    parameters:
    - name: global.environment
      value: production
    values: |
      replicas: 3
      image:
        tag: stable
```

#### **Helm Chart from HTTP Repository**
```yaml
source:
  repoURL: https://charts.helm.sh/stable      # Helm repository URL
  chart: postgresql                          # Chart name
  targetRevision: 12.1.5                    # Chart version
  helm:
    parameters:
    - name: auth.postgresPassword
      value: supersecret
    - name: primary.persistence.size
      value: 20Gi
```

#### **Helm with Custom Release Name**
```yaml
source:
  helm:
    releaseName: my-custom-release    # Custom Helm release name
    # If not specified, uses Application name
```

### **Kustomize Sources**

#### **Basic Kustomize**
```yaml
source:
  repoURL: https://github.com/myorg/kustomize-configs
  path: overlays/production
  targetRevision: HEAD
  kustomize:
    # Image replacements
    images:
    - myregistry.io/frontend:v1.2.3
    - myregistry.io/backend:v2.1.0
    
    # Replica overrides
    replicas:
    - name: frontend-deployment
      count: 5
    - name: backend-deployment  
      count: 3
```

#### **Advanced Kustomize Configuration**
```yaml
source:
  kustomize:
    # Image replacements with name matching
    images:
    - name: app                           # kustomization.yaml image name
      newName: myregistry.io/myapp       # New registry/name
      newTag: v1.2.3                    # New tag
    - name: nginx
      newName: myregistry.io/nginx
      digest: sha256:abc123...          # Pin to specific digest
      
    # Common labels applied to all resources
    commonLabels:
      app.kubernetes.io/version: v1.2.3
      environment: production
      team: backend
      
    # Common annotations
    commonAnnotations:
      config-hash: abc123def456
      deployed-by: argocd
      
    # Namespace override
    namespace: custom-namespace
    
    # Name prefix/suffix
    namePrefix: prod-
    nameSuffix: -v2
    
    # Strategic merge patches
    patchesStrategicMerge:
    - patches/deployment-patch.yaml
    - patches/service-patch.yaml
    
    # JSON 6902 patches
    patchesJson6902:
    - target:
        group: apps
        version: v1
        kind: Deployment
        name: my-deployment
      patch: |
        - op: replace
          path: /spec/replicas
          value: 5
        - op: add
          path: /spec/template/spec/containers/0/env/-
          value:
            name: ENVIRONMENT
            value: production
```

#### **Kustomize Build Options**
```yaml
source:
  kustomize:
    # Build options
    buildOptions: --enable-helm --load-restrictor LoadRestrictionsNone
    
    # Force common labels on all resources
    forceCommonLabels: true
    
    # Force common annotations on all resources  
    forceCommonAnnotations: true
```

## 🎯 Application Destination Configuration

### **Same Cluster Deployment**
```yaml
spec:
  destination:
    # Deploy to same cluster where Argo CD runs
    server: https://kubernetes.default.svc    # In-cluster server URL
    namespace: production                     # Target namespace
```

### **External Cluster Deployment**
```yaml
spec:
  destination:
    # Deploy to external/remote cluster
    server: https://prod-cluster.example.com  # External cluster API URL
    namespace: production
    
    # Alternative: use cluster name
    # name: production-cluster                # Cluster name (if registered)
    # namespace: production
```

### **Namespace Management**
```yaml
spec:
  destination:
    server: https://kubernetes.default.svc
    namespace: new-namespace
    
  syncPolicy:
    syncOptions:
    # Create namespace if it doesn't exist
    - CreateNamespace=true
    
    # Manage namespace metadata
    managedNamespaceMetadata:
      labels:
        environment: production
        team: backend
        app.kubernetes.io/managed-by: argocd
      annotations:
        argocd.argoproj.io/sync-wave: "-1"
```

## 🔄 Sync Policy Configuration

### **Manual Sync (Default)**
```yaml
spec:
  # No syncPolicy defined = manual sync only
  syncPolicy: {}
  
  # Explicit manual sync
  syncPolicy:
    automated: null    # Disable automated sync
    manual: true       # Optional, manual is default
```

### **Automated Sync**
```yaml
spec:
  syncPolicy:
    automated:
      # Basic automated sync
      prune: false              # Don't delete resources not in Git
      selfHeal: false          # Don't revert manual changes
      allowEmpty: false        # Prevent sync when no resources
      
    # Retry configuration
    retry:
      limit: 5                 # Max retry attempts
      backoff:
        duration: 5s          # Initial backoff duration
        factor: 2             # Backoff multiplier (5s, 10s, 20s, 40s, 80s)
        maxDuration: 3m       # Maximum backoff duration
```

### **Full Automated Sync with Pruning**
```yaml
spec:
  syncPolicy:
    automated:
      prune: true              # Delete resources not in Git
      selfHeal: true          # Revert manual cluster changes
      allowEmpty: false       # Prevent empty syncsstop
      
    syncOptions:
    # Validation and creation options
    - Validate=true           # Validate resources before apply
    - CreateNamespace=true    # Create namespace if missing
    - ApplyOutOfSyncOnly=true # Only apply out-of-sync resources
    
    # Pruning options  
    - PrunePropagationPolicy=foreground  # How to handle dependent resources
    - PruneLast=true         # Prune resources after main sync
    
    # Resource management
    - RespectIgnoreDifferences=true      # Honor ignoreDifferences
    - Replace=false          # Use kubectl apply, not replace
    
    # Server-side apply (Kubernetes 1.18+)
    - ServerSideApply=true   # Use server-side apply instead of client-side
    
    managedNamespaceMetadata:
      labels:
        managed-by: argocd
```

### **Conditional Sync Policies**
```yaml
spec:
  # Different sync policies can be applied based on branch
  source:
    targetRevision: HEAD
    
  syncPolicy:
    # Automated for main branch, manual for others
    automated:
      prune: true
      selfHeal: true
      
    # Sync options that can be conditional
    syncOptions:
    - CreateNamespace=true
    
  # Use ApplicationSet for branch-specific policies
```

## ⚙️ Advanced Application Features

### **Resource Customization**

#### **Ignore Differences**
```yaml
spec:
  # Ignore differences in specific fields
  ignoreDifferences:
  - group: apps
    kind: Deployment
    jsonPointers:
    - /spec/replicas                    # Ignore replica count changes
    - /spec/template/spec/containers/0/image  # Ignore image changes
    
  - group: ""
    kind: Service  
    jqPathExpressions:
    - '.spec.ports[]?.nodePort'         # Ignore nodePort assignments
    - '.metadata.annotations["kubectl.kubernetes.io/last-applied-configuration"]'
    
  - group: ""
    kind: ConfigMap
    name: my-configmap
    jsonPointers:
    - /data/dynamic-key                 # Ignore specific ConfigMap keys
```

#### **Resource Inclusions/Exclusions**
```yaml
spec:
  # Only sync specific resources
  resourceInclusions:
  - group: apps
    kind: Deployment
  - group: ""
    kind: Service
  - group: networking.k8s.io
    kind: Ingress
    
  # OR exclude specific resources
  resourceExclusions:
  - group: ""
    kind: Secret              # Don't manage secrets
  - group: batch
    kind: Job
    name: completed-job-*     # Exclude completed jobs
```

### **Sync Hooks y Waves**

#### **Resource Hooks**
```yaml
# PreSync hook - runs before main sync
apiVersion: batch/v1
kind: Job
metadata:
  name: database-migration
  annotations:
    argocd.argoproj.io/hook: PreSync
    argocd.argoproj.io/hook-delete-policy: BeforeHookCreation
    argocd.argoproj.io/sync-wave: "-1"
spec:
  template:
    spec:
      containers:
      - name: migrate
        image: migrate/migrate:latest
        command: ["migrate", "-database", "postgres://db:5432/myapp", "up"]
      restartPolicy: Never

---
# PostSync hook - runs after main sync
apiVersion: batch/v1
kind: Job  
metadata:
  name: send-notification
  annotations:
    argocd.argoproj.io/hook: PostSync
    argocd.argoproj.io/hook-delete-policy: HookSucceeded
    argocd.argoproj.io/sync-wave: "1"
spec:
  template:
    spec:
      containers:
      - name: notify
        image: curlimages/curl
        command: ["curl", "-X", "POST", "https://hooks.slack.com/...", "-d", "Deployment completed"]
      restartPolicy: Never

---  
# SyncFail hook - runs if sync fails
apiVersion: v1
kind: Pod
metadata:
  name: cleanup-on-failure
  annotations:
    argocd.argoproj.io/hook: SyncFail
    argocd.argoproj.io/hook-delete-policy: HookSucceeded
spec:
  containers:
  - name: cleanup
    image: kubectl:latest
    command: ["kubectl", "delete", "job", "failed-migration"]
  restartPolicy: Never
```

#### **Sync Waves**
```yaml
# Wave -2: Infrastructure
apiVersion: v1
kind: Namespace
metadata:
  name: myapp
  annotations:
    argocd.argoproj.io/sync-wave: "-2"

---
# Wave -1: Secrets and ConfigMaps
apiVersion: v1
kind: Secret
metadata:
  name: database-credentials
  annotations:
    argocd.argoproj.io/sync-wave: "-1"
type: Opaque
data:
  password: cGFzc3dvcmQ=

---
# Wave 0: Applications (default wave)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  annotations:
    argocd.argoproj.io/sync-wave: "0"    # Default, can be omitted
spec:
  replicas: 3
  # ...

---  
# Wave 1: Services
apiVersion: v1
kind: Service
metadata:
  name: backend-service
  annotations:
    argocd.argoproj.io/sync-wave: "1"
spec:
  # ...

---
# Wave 2: Ingress
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: backend-ingress
  annotations:
    argocd.argoproj.io/sync-wave: "2"
spec:
  # ...
```

### **Health Checks**

#### **Custom Health Checks**
```yaml
# Application-level health check configuration
spec:
  syncPolicy:
    syncOptions:
    - SkipDryRunOnMissingResource=true
    
# Custom resource health annotation
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  annotations:
    # Custom health check
    argocd.argoproj.io/health-check: |
      spec:
        containers:
        - name: my-container
          readinessProbe:
            httpGet:
              path: /health
              port: 8080
            initialDelaySeconds: 30
            periodSeconds: 10
```

#### **Resource-Specific Health Status**
```yaml
# Healthy resource indicators
Health Status:
  Deployment: Available replicas = Desired replicas
  Pod: Phase = Running, containers ready
  Service: Service exists, endpoints available  
  Ingress: Rules configured, TLS certificates valid
  Job: Completion status = Succeeded
  PersistentVolumeClaim: Status = Bound
  
# Custom health checks for CRDs
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
data:
  resource.customizations.health.example.com_widgets: |
    hs = {}
    if obj.status ~= nil then
      if obj.status.phase == "Ready" then
        hs.status = "Healthy"
        hs.message = "Widget is ready"
        return hs
      end
    end
    hs.status = "Progressing"
    hs.message = "Widget is not ready"
    return hs
```

## 📊 Application Examples

### **Simple Static Website**
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: static-website
  namespace: argocd
  finalizers:
  - resources-finalizer.argocd.argoproj.io
spec:
  project: default
  source:
    repoURL: https://github.com/myorg/static-website-config
    path: k8s/
    targetRevision: HEAD
  destination:
    server: https://kubernetes.default.svc
    namespace: websites  
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
```

### **Microservice with Helm**
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: user-service
  namespace: argocd
  finalizers:
  - resources-finalizer.argocd.argoproj.io
spec:
  project: microservices
  source:
    repoURL: https://github.com/myorg/helm-charts
    path: charts/microservice
    targetRevision: HEAD
    helm:
      valueFiles:
      - values.yaml
      - environments/production.yaml
      parameters:
      - name: image.repository
        value: myregistry.io/user-service
      - name: image.tag
        value: v2.1.0
      - name: replicaCount
        value: "5"
      - name: autoscaling.enabled  
        value: "true"
      - name: autoscaling.maxReplicas
        value: "10"
      values: |
        ingress:
          enabled: true
          hosts:
          - host: api.example.com
            paths:
            - path: /users
              pathType: Prefix
        resources:
          limits:
            memory: 512Mi
            cpu: 500m
          requests:
            memory: 256Mi
            cpu: 250m
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
    retry:
      limit: 3
      backoff:
        duration: 5s
        maxDuration: 3m
        factor: 2
```

### **Multi-Environment Application**  
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: backend-production
  namespace: argocd
spec:
  project: backend-team
  source:
    repoURL: https://github.com/myorg/backend-configs
    path: overlays/production
    targetRevision: main         # Production uses main branch
    kustomize:
      images:
      - myregistry.io/backend:stable
      replicas:
      - name: backend-deployment
        count: 10               # High replica count for production
      commonLabels:
        environment: production
        version: stable
  destination:
    server: https://prod-cluster.example.com
    namespace: backend-prod
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
    retry:
      limit: 5
      backoff:
        duration: 10s
        maxDuration: 5m
        factor: 2
  ignoreDifferences:
  - group: apps
    kind: Deployment
    jsonPointers:
    - /spec/replicas        # Allow HPA to manage replicas

---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: backend-staging
  namespace: argocd
spec:
  project: backend-team
  source:
    repoURL: https://github.com/myorg/backend-configs
    path: overlays/staging
    targetRevision: develop     # Staging uses develop branch
    kustomize:
      images:
      - myregistry.io/backend:latest
      replicas:
      - name: backend-deployment
        count: 2               # Lower replica count for staging
      commonLabels:
        environment: staging
        version: latest
  destination:
    server: https://staging-cluster.example.com
    namespace: backend-staging
  syncPolicy:
    automated:
      prune: true
      selfHeal: false         # Allow manual changes in staging
    syncOptions:
    - CreateNamespace=true
```

## 🚨 Common Configuration Issues

### **Source Configuration Problems**
```bash
# Issue: Repository not accessible
Status: ComparisonError
Message: "rpc error: code = Unknown desc = authentication required"

# Solutions:
# 1. Check repository credentials
argocd repo test https://github.com/myorg/configs

# 2. Verify repository URL
# 3. Check network connectivity from repo-server
```

### **Destination Configuration Problems**
```bash
# Issue: Cluster not accessible
Status: ComparisonError  
Message: "unable to create application in target cluster"

# Solutions:
# 1. Verify cluster connectivity
argocd cluster list
argocd cluster get https://kubernetes.default.svc

# 2. Check RBAC permissions
kubectl auth can-i create deployments --as=system:serviceaccount:argocd:argocd-application-controller -n target-namespace
```

### **Sync Policy Problems**
```bash
# Issue: Resources not being pruned
Status: Synced
Health: Healthy
# But old resources remain

# Solution: Enable pruning
syncPolicy:
  automated:
    prune: true

# Issue: Sync keeps failing/retrying  
Status: OutOfSync
Operation: Failed

# Solution: Check retry configuration
syncPolicy:
  retry:
    limit: 3          # Limit retries to avoid infinite loops
```

## 🎯 Exam Focus Points

### **Critical Configuration Knowledge**
1. **Application Structure**: metadata, source, destination, syncPolicy, project
2. **Source Types**: Git (plain YAML), Helm (Git/OCI/HTTP), Kustomize  
3. **Sync Policies**: manual, automated, prune, selfHeal
4. **Resource Management**: ignore differences, hooks, waves

### **Common Exam Questions**
- **"How to deploy Helm chart from OCI registry?"**
  ```yaml
  source:
    repoURL: oci://registry.example.com/charts
    chart: my-app
    targetRevision: 1.2.3
  ```

- **"How to ignore replica differences in Deployment?"**
  ```yaml
  ignoreDifferences:
  - group: apps
    kind: Deployment
    jsonPointers:
    - /spec/replicas
  ```

- **"How to run job before main deployment?"**
  ```yaml
  metadata:
    annotations:
      argocd.argoproj.io/hook: PreSync
      argocd.argoproj.io/sync-wave: "-1"
  ```

- **"How to create namespace automatically?"**
  ```yaml
  syncPolicy:
    syncOptions:
    - CreateNamespace=true
  ```

### **Troubleshooting Checklist**
- [ ] **Repository access** - URL, credentials, network connectivity
- [ ] **Cluster access** - Server URL, authentication, RBAC
- [ ] **Namespace permissions** - Controller can create/update resources
- [ ] **Sync policy conflicts** - Manual vs automated, prune settings
- [ ] **Resource validation** - YAML syntax, Kubernetes API compatibility

## 📚 Next Topics

1. [06 - AppProjects y Multi-tenancy](06-appprojects-multitenancy.md) 
2. [09 - Sync Policies](09-sync-policies.md)
3. [13 - Helm Integration](13-helm-integration.md)