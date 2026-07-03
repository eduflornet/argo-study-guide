# 🏗️ Arquitectura de Argo CD

## ¿Qué es Argo CD?

**Argo CD** es un **GitOps continuous delivery controller** para Kubernetes que **automatically synchronizes** el estado de aplicaciones entre **Git repositories** y **Kubernetes clusters**, implementando el patrón **"Git como fuente única de verdad"**.

## 🧩 Componentes Principales

### **Arquitectura Completa**

```mermaid
graph TB
    subgraph "External Interfaces"
        A[argocd CLI]
        B[Web UI]
        C[Git Repositories]
        D[External Clusters]
        E[OIDC Provider]
        F[Webhook Sources]
    end
    
    subgraph "Argo CD Control Plane"
        subgraph "API Layer"
            G[API Server]
            H[gRPC Gateway]
        end
        
        subgraph "Core Services"
            I[Repository Server]
            J[Application Controller]
            K[ApplicationSet Controller]
            L[Notification Controller]
        end
        
        subgraph "Data Layer"
            M[Redis Cache]
            N[etcd/K8s API]
        end
    end
    
    subgraph "Target Clusters"
        O[Local Cluster]
        P[Remote Cluster 1]
        Q[Remote Cluster 2]
    end
    
    A --> G
    B --> H
    H --> G
    E --> G
    F --> L
    
    C --> I
    G --> I
    G --> J
    G --> K
    
    I --> M
    J --> N
    K --> J
    
    J --> O
    J --> P
    J --> Q
    D --> P
    D --> Q
```

## 🎯 1. API Server

**Frontend component** que expone **gRPC/REST API** para todas las interacciones con Argo CD.

### **Responsabilidades**
```yaml
API Server:
  Authentication: ✅ User login, JWT tokens, RBAC
  Authorization: ✅ RBAC policy enforcement
  Application Management: ✅ CRUD operations on Applications
  Repository Management: ✅ Git repo configuration
  Cluster Management: ✅ External cluster registration
  Project Management: ✅ AppProject CRUD operations
  Sync Operations: ✅ Manual sync triggers
  UI/CLI Gateway: ✅ Web UI and CLI backend
```

### **Deployment Configuration**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: argocd-server
  namespace: argocd
spec:
  replicas: 2          # HA deployment
  template:
    spec:
      containers:
      - name: argocd-server
        image: quay.io/argoproj/argocd:v2.8.0
        command:
        - argocd-server
        - --staticassets
        - /shared/app
        ports:
        - containerPort: 8080    # gRPC API
          name: server
        - containerPort: 8083    # Metrics
          name: metrics
        env:
        - name: ARGOCD_SERVER_INSECURE
          value: "false"
        livenessProbe:
          httpGet:
            path: /healthz
            port: 8080
          initialDelaySeconds: 10
          periodSeconds: 30
        readinessProbe:
          httpGet:
            path: /healthz
            port: 8080
```

### **Service Configuration**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: argocd-server
  namespace: argocd
spec:
  type: LoadBalancer    # or ClusterIP with Ingress
  ports:
  - port: 80
    targetPort: 8080
    name: server
  - port: 443
    targetPort: 8080
    name: grpc
  selector:
    app.kubernetes.io/name: argocd-server
```

### **API Server Interactions**
```bash
# CLI interactions
argocd login server.example.com
argocd app list
argocd app sync my-app

# REST API calls (from CI/CD)
curl -H "Authorization: Bearer $TOKEN" \
  https://argocd.example.com/api/v1/applications

# Web UI access
https://argocd.example.com/applications
```

## 📦 2. Repository Server

**Git repository backend** que **clones, caches y generates manifests** desde multiple source types.

### **Responsabilidades**
```yaml
Repository Server:
  Git Operations: ✅ Clone, fetch, cache repositories
  Manifest Generation: ✅ Plain YAML, Helm, Kustomize processing
  Plugin Execution: ✅ Custom config management tools
  Credential Management: ✅ Git authentication (SSH, HTTPS, tokens)
  Cache Management: ✅ Repository and manifests caching
  Security: ✅ Git credential isolation
```

### **Supported Source Types**
```yaml
# Plain Kubernetes YAML
source:
  repoURL: https://github.com/org/k8s-configs
  path: manifests/
  targetRevision: HEAD

# Helm Charts
source:
  repoURL: https://github.com/org/helm-charts
  path: charts/my-app
  targetRevision: HEAD
  helm:
    valueFiles: [values.yaml, values-prod.yaml]
    parameters:
    - name: image.tag
      value: v1.2.3

# Kustomize
source:
  repoURL: https://github.com/org/kustomized-configs
  path: overlays/production
  targetRevision: HEAD
  kustomize:
    images: [myregistry/myapp:v1.2.3]

# OCI Helm Charts
source:
  repoURL: oci://registry.example.com/charts
  chart: my-app
  targetRevision: 1.2.3

# Git Submodules
source:
  repoURL: https://github.com/org/main-config
  path: submodules/app-config
  targetRevision: HEAD
```

### **Cache Configuration**
```yaml
# Repository server with cache tuning
containers:
- name: argocd-repo-server
  env:
  - name: HELM_CACHE_HOME
    value: /helm-working-dir
  - name: HELM_CONFIG_HOME  
    value: /helm-working-dir
  - name: KUSTOMIZE_PLUGIN_HOME
    value: /kustomize-plugins
  volumeMounts:
  - name: git-cache
    mountPath: /app/config/ssh
  - name: helm-cache
    mountPath: /helm-working-dir
  resources:
    requests:
      memory: 256Mi
      cpu: 100m
    limits:
      memory: 1Gi
      cpu: 500m
```

### **Git Repository Access**
```yaml
# HTTPS with token
apiVersion: v1
kind: Secret
metadata:
  name: repo-https-creds
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
type: Opaque
stringData:
  type: git
  url: https://github.com/org/configs
  username: oauth2
  password: ghp_xxxxxxxxxxxxxxxxxxxx

# SSH key authentication
apiVersion: v1
kind: Secret
metadata:
  name: repo-ssh-creds
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
type: Opaque
stringData:
  type: git
  url: git@github.com:org/configs.git
  sshPrivateKey: |
    -----BEGIN OPENSSH PRIVATE KEY-----
    ...
    -----END OPENSSH PRIVATE KEY-----
```

## 🎮 3. Application Controller

**Core reconciliation engine** que **continuously monitors applications** y **synchronizes desired state**.

### **Responsabilidades**
```yaml
Application Controller:
  Continuous Monitoring: ✅ Poll Git repos for changes
  State Comparison: ✅ Git content vs Cluster content
  Sync Execution: ✅ kubectl apply/delete operations
  Health Assessment: ✅ Resource health monitoring
  Auto-Sync: ✅ Automated synchronization
  Self-Healing: ✅ Revert manual cluster changes
  Pruning: ✅ Delete resources not in Git
  Hook Execution: ✅ PreSync, Sync, PostSync hooks
  Resource Ordering: ✅ Sync waves and phases
```

### **Reconciliation Loop**
```mermaid
stateDiagram-v2
    [*] --> Watching
    Watching --> Detecting: Git Change Detected
    Watching --> Detecting: Health Check Timer
    Detecting --> Comparing: Fetch Desired State
    Comparing --> Synced: States Match
    Comparing --> OutOfSync: States Differ
    Synced --> Watching: Continue Monitoring
    OutOfSync --> SyncRequired: Auto-Sync Enabled
    OutOfSync --> Watching: Manual Sync Only
    SyncRequired --> Syncing: Execute Sync
    Syncing --> Synced: Sync Success
    Syncing --> Degraded: Sync Failed
    Degraded --> SyncRequired: Retry Logic
    Synced --> HealthCheck: Check Resource Health
    HealthCheck --> Healthy: All Resources OK
    HealthCheck --> Degraded: Some Resources Failed
    Healthy --> Watching
    Degraded --> Watching
```

### **Controller Configuration**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: argocd-application-controller
spec:
  replicas: 1          # Single replica (leader election)
  template:
    spec:
      containers:
      - name: application-controller
        command:
        - argocd-application-controller
        - --status-processors
        - "20"             # Concurrent status processors
        - --operation-processors  
        - "10"             # Concurrent operation processors
        - --app-resync
        - "180"            # App resync interval (seconds)
        - --repo-server
        - argocd-repo-server:8081
        - --redis
        - argocd-redis:6379
        env:
        - name: ARGOCD_CONTROLLER_REPLICAS
          value: "1"
        - name: ARGOCD_HARD_RECONCILIATION_TIMEOUT
          value: "0"       # Disable hard reconciliation timeout
        resources:
          requests:
            memory: 1Gi
            cpu: 250m
          limits:
            memory: 2Gi
            cpu: 500m
```

### **Application States**
```yaml
# Application sync status
Status:
  Sync: 
    - Synced      # Git content = Cluster content
    - OutOfSync   # Git content ≠ Cluster content
    - Unknown     # Unable to determine sync status
    
  Health:
    - Healthy     # All resources are healthy
    - Progressing # Sync in progress, resources starting
    - Degraded    # Some resources are unhealthy
    - Suspended   # Application is suspended
    - Missing     # Resources are missing
    - Unknown     # Unable to determine health
    
  Operation:
    - Running     # Sync operation in progress
    - Failed      # Sync operation failed
    - Error       # Sync operation had errors
    - Succeeded   # Sync operation completed successfully
```

## 🚀 4. ApplicationSet Controller

**Multi-application generator** que **automatically creates and manages** multiple Applications from templates.

### **Responsabilidades**
```yaml
ApplicationSet Controller:
  Template Processing: ✅ Generate Applications from templates
  Multi-cluster Support: ✅ Deploy to multiple clusters
  Dynamic Discovery: ✅ Git directories, Cluster API, Pull Requests
  Auto-generation: ✅ Continuous Application creation
  Lifecycle Management: ✅ Create, update, delete Applications
```

### **ApplicationSet Example**
```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: microservices
  namespace: argocd
spec:
  generators:
  # Generate app for each directory in Git repo
  - git:
      repoURL: https://github.com/org/microservices
      revision: HEAD
      directories:
      - path: services/*
  
  # Generate app for each cluster
  - clusters: {}
  
  template:
    metadata:
      name: '{{path.basename}}-{{name}}'
    spec:
      project: default
      source:
        repoURL: https://github.com/org/microservices
        targetRevision: HEAD
        path: '{{path}}'
      destination:
        server: '{{server}}'
        namespace: '{{path.basename}}'
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
        syncOptions:
        - CreateNamespace=true
```

## 📢 5. Notification Controller

**Event notification system** que **sends notifications** about application events.

### **Notification Types**
```yaml
# Slack notification
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-notifications-cm
  namespace: argocd
data:
  service.slack: |
    token: xoxb-slack-bot-token
  template.app-deployed: |
    message: |
      Application {{.app.metadata.name}} deployed to {{.app.spec.destination.server}}
  trigger.on-deployed: |
    - when: app.status.operationState.phase in ['Succeeded'] and app.status.health.status == 'Healthy'
      send: [app-deployed]

# Email notification  
  service.email: |
    host: smtp.gmail.com
    port: 587
    from: argocd@company.com
    username: argocd@company.com
  template.app-sync-failed: |
    subject: Application {{.app.metadata.name}} sync failed
    body: |
      Application: {{.app.metadata.name}}
      Sync Status: {{.app.status.sync.status}}
      Health Status: {{.app.status.health.status}}
      Repository: {{.app.spec.source.repoURL}}
```

## 💾 6. Redis Cache

**In-memory cache** para **performance optimization** y **temporary data storage**.

### **Cache Usage**
```yaml
Redis Cache:
  Repository Cache: ✅ Git commits, manifests
  Application Cache: ✅ Application states, resources
  Session Cache: ✅ User sessions, tokens  
  Lock Management: ✅ Operation locks, leader election
  Publish/Subscribe: ✅ Real-time notifications
```

### **Redis Configuration**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: argocd-redis
spec:
  template:
    spec:
      containers:
      - name: redis
        image: redis:7-alpine
        command:
        - redis-server
        - --save
        - ""                    # Disable persistence
        - --appendonly
        - "no"                  # Disable AOF
        - --maxmemory
        - "256mb"               # Memory limit
        - --maxmemory-policy
        - "allkeys-lru"         # Eviction policy
        ports:
        - containerPort: 6379
        resources:
          requests:
            memory: 64Mi
            cpu: 50m
          limits:
            memory: 256Mi
            cpu: 200m
```

## 🔄 Component Communication Flow

### **Application Sync Flow**
```mermaid
sequenceDiagram
    participant U as User/CLI
    participant API as API Server
    participant C as App Controller
    participant R as Repo Server
    participant G as Git Repository
    participant K as Kubernetes Cluster
    
    U->>API: argocd app sync my-app
    API->>C: Trigger sync operation
    C->>R: Request desired manifests
    R->>G: Git clone/fetch
    G-->>R: Repository content
    R->>R: Generate manifests (Helm/Kustomize)
    R-->>C: Return manifests
    C->>C: Compare desired vs actual state
    C->>K: kubectl apply manifests
    K-->>C: Apply results
    C->>API: Update operation status
    API-->>U: Sync completed
```

### **Health Check Flow**
```mermaid
sequenceDiagram
    participant C as App Controller
    participant K as Kubernetes API
    participant R as Resources
    
    loop Every 15 seconds
        C->>K: Get application resources
        K-->>C: Resource definitions
        C->>R: Check resource health
        R-->>C: Health status
        C->>C: Aggregate health status
        C->>K: Update Application status
    end
```

## 🏗️ Deployment Architectures

### **Single Cluster Deployment**
```yaml
# Standard deployment in single cluster
apiVersion: v1
kind: Namespace
metadata:
  name: argocd

---
# All Argo CD components in same cluster
apiVersion: apps/v1
kind: Deployment
metadata:
  name: argocd-server
  namespace: argocd
# ... API Server configuration

---
apiVersion: apps/v1  
kind: Deployment
metadata:
  name: argocd-application-controller
  namespace: argocd
# ... Controller configuration

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: argocd-repo-server
  namespace: argocd
# ... Repository Server configuration
```

### **Multi-Cluster Deployment**
```yaml
# Argo CD in management cluster
# Deploys to multiple external clusters

# External cluster registration
argocd cluster add prod-cluster \
  --kubeconfig ~/.kube/prod-config \
  --name production

# Application targeting external cluster
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: prod-app
  namespace: argocd
spec:
  destination:
    server: https://prod-cluster.example.com    # External cluster
    namespace: production
  source:
    repoURL: https://github.com/org/prod-configs
    path: apps/
```

### **High Availability Deployment**
```yaml
# HA API Server
apiVersion: apps/v1
kind: Deployment
metadata:
  name: argocd-server
spec:
  replicas: 3              # Multiple replicas
  template:
    spec:
      affinity:
        podAntiAffinity:   # Spread across nodes
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchLabels:
                app.kubernetes.io/name: argocd-server
            topologyKey: kubernetes.io/hostname

# HA Repository Server
apiVersion: apps/v1
kind: Deployment
metadata:
  name: argocd-repo-server
spec:
  replicas: 3              # Stateless, can scale

# Application Controller remains single replica
# Uses leader election for HA
```

## 🔧 Configuration Examples

### **Repository Configuration**
```yaml
# Git repository credentials
apiVersion: v1
kind: Secret
metadata:
  name: production-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/company/production-configs
  username: deploy-token
  password: token_value

# Helm repository
apiVersion: v1
kind: Secret
metadata:
  name: helm-charts
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: helm
  url: https://charts.company.com/stable
  username: helm-user
  password: helm-pass
```

### **Cluster Configuration**
```yaml
# External cluster credentials
apiVersion: v1
kind: Secret
metadata:
  name: staging-cluster
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: cluster
stringData:
  name: staging
  server: https://staging.k8s.company.com
  config: |
    {
      "bearerToken": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
      "tlsClientConfig": {
        "insecure": false,
        "caData": "LS0tLS1CRUdJTi..."
      }
    }
```

## 📊 Performance Considerations

### **Resource Requirements**
```yaml
# API Server
resources:
  requests:
    memory: 128Mi
    cpu: 100m
  limits:
    memory: 512Mi
    cpu: 500m

# Application Controller  
resources:
  requests:
    memory: 1Gi        # Higher memory for large deployments
    cpu: 250m
  limits:
    memory: 2Gi
    cpu: 500m

# Repository Server
resources:
  requests:
    memory: 256Mi      # Cache requirements
    cpu: 100m
  limits:
    memory: 1Gi
    cpu: 500m

# Redis
resources:
  requests:
    memory: 64Mi
    cpu: 50m
  limits:
    memory: 256Mi
    cpu: 200m
```

### **Scaling Guidelines**
```yaml
Scaling Factors:
  Applications: 1000+ apps → Scale repo server replicas
  Repositories: 100+ repos → Increase repo server memory
  Clusters: 10+ clusters → Scale API server replicas
  Sync Operations: High frequency → Increase controller processors
  Users: 100+ users → Scale API server replicas + Redis memory
```

## 🚨 Troubleshooting Architecture

### **Component Health Checks**
```bash
# Check all Argo CD components
kubectl get pods -n argocd

# API Server health
kubectl port-forward -n argocd svc/argocd-server 8080:80
curl http://localhost:8080/healthz

# Repository Server health  
kubectl logs -n argocd deployment/argocd-repo-server

# Application Controller health
kubectl logs -n argocd deployment/argocd-application-controller

# Redis connectivity
kubectl exec -n argocd deployment/argocd-redis -- redis-cli ping
```

### **Common Issues y Solutions**
```bash
# Repository Server out of memory
# Solution: Increase memory limits, clear cache
kubectl delete pods -n argocd -l app.kubernetes.io/name=argocd-repo-server

# Application Controller sync failures
# Solution: Check RBAC, network policies, resource limits
kubectl logs -n argocd deployment/argocd-application-controller -f

# API Server authentication issues
# Solution: Check OIDC configuration, certificates
kubectl describe configmap argocd-cmd-params-cm -n argocd

# Redis connection timeouts
# Solution: Check network policies, increase Redis memory
kubectl exec -n argocd deployment/argocd-redis -- redis-cli info memory
```

## 🎯 Exam Focus Points

### **Critical Architecture Knowledge**
1. **Component Roles**: API Server (frontend), Repo Server (Git), Controller (reconciliation)
2. **Communication Flow**: User → API → Controller → Repo → Git → Kubernetes
3. **Scaling Strategy**: API Server (stateless), Controller (single), Repo Server (stateless)
4. **Cache Usage**: Redis for performance, temporary data

### **Common Exam Questions**
- **"Which component interacts with Git repositories?"** → Repository Server
- **"Which component performs application synchronization?"** → Application Controller
- **"How many Application Controller replicas should run?"** → One (uses leader election)
- **"Where is the Argo CD UI served from?"** → API Server

### **Troubleshooting Scenarios**
- **Sync failures** → Check Application Controller logs
- **Manifest generation issues** → Check Repository Server logs
- **Authentication problems** → Check API Server configuration
- **Performance issues** → Check Redis and resource limits

## 📚 Next Steps

1. [05 - Application Configuration](05-application-configuration.md)
2. [09 - Sync Policies](09-sync-policies.md)
3. [26 - Troubleshooting Guide](26-troubleshooting-guide.md)

-   **Responsabilidades**:
    -   Almacenar en caché el estado de las aplicaciones para evitar sobrecargar la API de Kubernetes.
    -   Guardar los resultados de la comparación de manifiestos.
    -   Invalidar la caché cuando se detectan cambios en Git o en el clúster.

-   **Examen**: Aunque es un componente de soporte, es bueno saber que existe y su propósito es mejorar el rendimiento.

---

### Flujo de Datos

1.  Un usuario (o un sistema de CI) interactúa con el **API Server** para crear una `Application`.
2.  El **Application Controller** detecta la nueva `Application`.
3.  El Application Controller le pide al **Repository Server** que clone el repositorio y genere los manifiestos.
4.  El Repository Server devuelve los manifiestos generados.
5.  El Application Controller compara estos manifiestos con lo que hay en el clúster de destino.
6.  Si hay diferencias, el Application Controller aplica los cambios para sincronizar el estado.
7.  El estado de la aplicación se almacena en **Redis** y se expone a través del **API Server**.