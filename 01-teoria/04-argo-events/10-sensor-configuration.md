# ⚡ Sensor Configuration

## ¿Qué es un Sensor?

Un **Sensor** es el componente de Argo Events que **procesa eventos** del EventBus, aplica **lógica de negocio** (filters, dependencies, conditions) y **ejecuta acciones** (triggers) como respuesta a eventos.

## 🧩 Anatomía de un Sensor

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Sensor
metadata:
  name: example-sensor
  namespace: argo-events
spec:
  # 1. What events to listen for
  dependencies:
  - name: event-dependency
    eventSourceName: source-name
    eventName: event-name
    filters: {...}
    
  # 2. How to evaluate dependencies (optional)
  dependencyLogic: "dependency1 && dependency2"
  
  # 3. What actions to execute
  triggers:
  - template:
      name: trigger-name
      argoWorkflow: {...}
      
  # 4. Error handling (optional)
  errorOnFailure: true
  
  # 5. Pod configuration (optional)
  template: {...}
```

## 📋 Dependencies: Event Listening

### **Single Dependency**
```yaml
spec:
  dependencies:
  - name: github-push        # Unique dependency name
    eventSourceName: github-source  # EventSource reference
    eventName: github-webhook       # Event type from EventSource
```

### **Multiple Dependencies**
```yaml
spec:
  dependencies:
  # Code change event
  - name: code-change
    eventSourceName: github-source
    eventName: github-webhook
    
  # Test results event  
  - name: test-results
    eventSourceName: ci-webhook
    eventName: test-completed
    
  # Security scan event
  - name: security-clear
    eventSourceName: security-webhook
    eventName: scan-passed
```

### **Dependency Logic**

#### **Default AND Logic**
```yaml
# Without explicit dependencyLogic
dependencies:
- name: event-a
- name: event-b  
- name: event-c

# Triggers execute when ALL dependencies are satisfied
# Logic: event-a AND event-b AND event-c
```

#### **Custom Logic Expressions**
```yaml
dependencies:
- name: code-push
- name: tests-passed
- name: security-clear
- name: manual-override

# Complex logic
dependencyLogic: "(code-push && tests-passed && security-clear) || manual-override"

# OR logic
dependencyLogic: "code-push || manual-trigger"

# Partial requirements
dependencyLogic: "code-push && (tests-passed || manual-approval)"
```

## 🔍 Filters: Event Filtering

### **Data Filters**
Filtrar por contenido del evento.

#### **String Filters**
```yaml
dependencies:
- name: github-main-push
  eventSourceName: github-source
  eventName: github-webhook
  filters:
    data:
    # Filter by branch
    - path: body.ref
      type: string
      value: ["refs/heads/main"]
      
    # Filter by GitHub event type
    - path: headers.X-Github-Event
      type: string
      value: ["push"]
      
    # Exclude specific authors
    - path: body.head_commit.author.login
      type: string
      comparator: "!="
      value: ["dependabot[bot]"]
```

#### **Regex Filters**
```yaml
filters:
  data:
  # Feature branches only
  - path: body.ref
    type: string
    comparator: "regexp"
    value: ["refs/heads/feature/.*"]
    
  # Commit messages with specific patterns
  - path: body.head_commit.message
    type: string  
    comparator: "regexp"
    value: ["^(feat|fix|deploy):.*"]
```

#### **Number Filters**
```yaml
filters:
  data:
  # Only if more than 1 file changed
  - path: body.commits.#
    type: number
    comparator: ">"
    value: ["1"]
    
  # Large pull requests  
  - path: body.pull_request.changed_files
    type: number
    comparator: ">="
    value: ["10"]
```

#### **Array/Nested Filters**
```yaml
filters:
  data:
  # Files with specific extensions  
  - path: body.commits.#.modified.#
    type: string
    value: ["*.yaml", "*.yml", "Dockerfile"]
    
  # Pull request labels
  - path: body.pull_request.labels.#.name
    type: string
    value: ["ready-for-review", "approved"]
```

### **Context Filters**
Filtrar por metadata del evento.

```yaml
dependencies:
- name: webhook-event
  eventSourceName: webhook-source
  eventName: webhook
  filters:
    context:
      # Event type
      type: webhook
      
      # Event source
      source: github-webhook
      
      # Event subject
      subject: "github.com/org/repo"
      
      # Time window
      time:
        start: "2024-01-01T00:00:00Z"
        stop: "2024-12-31T23:59:59Z"
```

### **Expression Filters**
Lógica compleja con expresiones.

```yaml
filters:
  expression: |
    has(body.pull_request) && 
    body.pull_request.state == "open" &&
    body.pull_request.base.ref == "main" &&
    body.action in ["opened", "synchronize"] &&
    body.pull_request.user.login != "dependabot[bot]" &&
    (
      has(body.pull_request.labels) &&
      body.pull_request.labels.#.name ?== "ready-for-review"
    )
```

## ⚡ Triggers: Action Execution

### **Argo Workflow Triggers**

#### **Basic Workflow Trigger**
```yaml  
triggers:
- template:
    name: build-trigger
    argoWorkflow:
      operation: submit       # submit | create | apply
      source:
        resource:
          apiVersion: argoproj.io/v1alpha1
          kind: Workflow
          metadata:
            generateName: build-workflow-
            namespace: argo
          spec:
            entrypoint: build
            arguments:
              parameters:
              - name: repo-url
                value: "{{.Input.github-push.body.repository.clone_url}}"
              - name: commit-sha
                value: "{{.Input.github-push.body.after}}"
```

#### **Conditional Workflow Trigger**
```yaml
triggers:
- template:
    name: production-deploy
    # Only trigger for main branch with deploy tag
    conditions: |
      request.body.ref == "refs/heads/main" && 
      request.body.head_commit.message | contains "deploy:"
    argoWorkflow:
      operation: submit
      source:
        resource:
          apiVersion: argoproj.io/v1alpha1
          kind: Workflow
          metadata:
            generateName: prod-deploy-
          spec:
            entrypoint: production-deployment
```

#### **Parameterized Workflow Trigger**
```yaml
triggers:
- template:
    name: parameterized-workflow
    argoWorkflow:
      operation: submit
      # Map event data to workflow parameters
      parameters:
      - src:
          dependencyName: github-push
          dataKey: body.repository.name
        dest: repo-name
      - src:
          dependencyName: github-push
          dataKey: body.ref
          # Transform refs/heads/main → main
        dest: branch
        operation: "substr 11"
      - src:
          value: "{{.Input.github-push.body.commits | length}}"
        dest: commit-count
      source:
        resource:
          # Workflow definition with parameters
          spec:
            arguments:
              parameters:
              - name: repo-name
              - name: branch  
              - name: commit-count
```

### **Kubernetes Resource Triggers**

#### **Create Resource Trigger**
```yaml
triggers:
- template:
    name: create-configmap
    k8s:
      operation: create
      source:
        resource:
          apiVersion: v1
          kind: ConfigMap
          metadata:
            name: "event-data-{{.Input.github-push.body.after | substr 0 7}}"
            namespace: default
            labels:
              event-source: github
              commit-sha: "{{.Input.github-push.body.after}}"
          data:
            repository: "{{.Input.github-push.body.repository.full_name}}"
            branch: "{{.Input.github-push.body.ref | substr 11}}"
            commit-message: "{{.Input.github-push.body.head_commit.message}}"
            event-time: "{{.Input.github-push.context.eventTime}}"
```

#### **Update Resource Trigger**
```yaml
triggers:
- template:
    name: update-deployment
    k8s:
      operation: patch
      patchStrategy: "strategic"    # strategic | merge | json
      source:
        resource:
          apiVersion: apps/v1
          kind: Deployment
          metadata:
            name: my-app
            namespace: production
          spec:
            template:
              metadata:
                annotations:
                  # Update with new commit info
                  deploy.commit: "{{.Input.github-push.body.after}}"
                  deploy.timestamp: "{{.Input.github-push.context.eventTime}}"
              spec:
                containers:
                - name: app
                  image: "myregistry/myapp:{{.Input.github-push.body.after | substr 0 7}}"
```

### **HTTP Triggers**

#### **REST API Call Trigger**
```yaml
triggers:
- template:
    name: api-notification
    http:
      url: "https://api.company.com/deployments"
      method: POST
      headers:
        Content-Type: application/json
        Authorization: "Bearer {{.Secrets.api-token-secret.token}}"
      payload:
      - src:
          value: |
            {
              "event": "deployment.triggered",
              "repository": "{{.Input.github-push.body.repository.full_name}}",
              "branch": "{{.Input.github-push.body.ref | substr 11}}",
              "commit": "{{.Input.github-push.body.after}}",
              "author": "{{.Input.github-push.body.head_commit.author.name}}",
              "timestamp": "{{.Input.github-push.context.eventTime}}"
            }
        dest: ""
      timeout: 30s
      # Retry configuration
      retryStrategy:
        steps: 3
        duration: 10s
```

#### **Slack Notification Trigger**
```yaml
triggers:
- template:
    name: slack-notification
    http:
      url: ""
      method: POST
      headers:
        Content-Type: application/json
      payload:
      - src:
          value: |
            {
              "channel": "#deployments", 
              "username": "Argo Events",
              "icon_emoji": ":rocket:",
              "text": "🚀 Deployment triggered for *{{.Input.github-push.body.repository.name}}*",
              "attachments": [
                {
                  "color": "good",
                  "fields": [
                    {
                      "title": "Repository",
                      "value": "{{.Input.github-push.body.repository.full_name}}",
                      "short": true
                    },
                    {
                      "title": "Branch", 
                      "value": "{{.Input.github-push.body.ref | substr 11}}",
                      "short": true
                    },
                    {
                      "title": "Commit SHA",
                      "value": "{{.Input.github-push.body.after | substr 0 7}}",
                      "short": true
                    },
                    {
                      "title": "Author",
                      "value": "{{.Input.github-push.body.head_commit.author.name}}",
                      "short": true
                    },
                    {
                      "title": "Commit Message",
                      "value": "{{.Input.github-push.body.head_commit.message}}",
                      "short": false
                    }
                  ]
                }
              ]
            }
        dest: ""
```

### **Kafka Message Triggers**
```yaml
triggers:
- template:
    name: kafka-message
    kafka:
      url: kafka.argo:9092
      topic: deployment-events
      partition: 0
      requiredAcks: 1
      compress: true
      flushFrequency: 2
      parameters:
      - src:
          value: |
            {
              "eventType": "deployment.triggered",
              "source": "github",
              "data": {
                "repository": "{{.Input.github-push.body.repository.full_name}}",
                "branch": "{{.Input.github-push.body.ref | substr 11}}",
                "commit": "{{.Input.github-push.body.after}}",
                "timestamp": "{{.Input.github-push.context.eventTime}}"
              }
            }
        dest: message
```

## 🔄 Complex Sensor Patterns

### **Multi-Environment Deployment**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Sensor
metadata:
  name: multi-env-deployment-sensor
spec:
  dependencies:
  - name: github-push
    eventSourceName: github-source
    eventName: github-webhook
    filters:
      data:
      - path: body.ref
        type: string
        value: ["refs/heads/main", "refs/heads/develop", "refs/heads/staging"]
        
  triggers:
  # Development environment
  - template:
      name: deploy-to-dev
      conditions: |
        request.body.ref == "refs/heads/develop"
      argoWorkflow:
        operation: submit
        source:
          resource:
            apiVersion: argoproj.io/v1alpha1
            kind: Workflow
            metadata:
              generateName: deploy-dev-
            spec:
              entrypoint: deploy
              arguments:
                parameters:
                - name: environment
                  value: "development"
                - name: replicas
                  value: "1"
                - name: resources
                  value: "minimal"
                  
  # Staging environment
  - template:
      name: deploy-to-staging
      conditions: |
        request.body.ref == "refs/heads/staging"
      argoWorkflow:
        # Staging deployment workflow
        
  # Production environment (with additional checks)
  - template:
      name: deploy-to-production
      conditions: |
        request.body.ref == "refs/heads/main" &&
        request.body.head_commit.message | contains "deploy:" &&
        !contains(request.body.head_commit.message, "[skip-deploy]")
      argoWorkflow:
        # Production deployment workflow
```

### **Pipeline with Approval Gates**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Sensor
metadata:
  name: approval-pipeline-sensor
spec:
  dependencies:
  # Code change
  - name: code-push
    eventSourceName: github-source
    eventName: github-webhook
    filters:
      data:
      - path: body.ref
        value: ["refs/heads/main"]
        
  # CI completion
  - name: ci-complete
    eventSourceName: ci-webhook
    eventName: ci-finished
    filters:
      data:
      - path: body.status
        value: ["success"]
        
  # Manual approval  
  - name: deployment-approval
    eventSourceName: approval-webhook
    eventName: approved
    
  # All three events required
  dependencyLogic: "code-push && ci-complete && deployment-approval"
  
  triggers:
  - template:
      name: approved-deployment
      argoWorkflow:
        # Deployment only after all gates passed
```

### **Microservices Orchestration**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Sensor
metadata:
  name: microservices-orchestration
spec:
  dependencies:
  # Service A deployment
  - name: service-a-deployed
    eventSourceName: deployment-webhook
    eventName: deployment-complete
    filters:
      data:
      - path: body.service
        value: ["service-a"]
      - path: body.status  
        value: ["success"]
        
  # Service B deployment
  - name: service-b-deployed
    eventSourceName: deployment-webhook  
    eventName: deployment-complete
    filters:
      data:
      - path: body.service
        value: ["service-b"]
      - path: body.status
        value: ["success"]
        
  triggers:
  # Integration tests (after both services deployed)
  - template:
      name: integration-tests
      argoWorkflow:
        operation: submit
        source:
          resource:  
            spec:
              entrypoint: integration-tests
              arguments:
                parameters:
                - name: service-a-version
                  value: "{{.Input.service-a-deployed.body.version}}"
                - name: service-b-version
                  value: "{{.Input.service-b-deployed.body.version}}"
                  
  # Update API Gateway routing
  - template:
      name: update-gateway
      k8s:
        operation: patch
        source:
          resource:
            apiVersion: v1
            kind: ConfigMap  
            metadata:
              name: api-gateway-config
            data:
              service-a-endpoint: "{{.Input.service-a-deployed.body.endpoint}}"
              service-b-endpoint: "{{.Input.service-b-deployed.body.endpoint}}"
```

## 🔧 Sensor Configuration

### **Resource Management**
```yaml
spec:
  # Pod template for sensor
  template:
    metadata:
      labels:
        app: custom-sensor
    spec:
      serviceAccountName: sensor-sa
      containers:
      - name: sensor
        resources:
          requests:
            memory: "64Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "500m"
        env:
        - name: DEBUG
          value: "true"
```

### **Error Handling**
```yaml
spec:
  # Stop executing triggers on first failure
  errorOnFailure: true
  
  triggers:
  - template:
      name: resilient-trigger
      # Retry configuration
      retryStrategy:
        steps: 3              # Max retry attempts
        duration: 10s         # Initial delay
        backoff:
          duration: 2s        # Backoff initial duration 
          factor: 2           # Backoff factor (2s, 4s, 8s)
          maxDuration: 60s    # Max backoff duration
      argoWorkflow:
        # Workflow definition
```

### **Scaling Configuration**
```yaml
spec:
  # Multiple sensor replicas
  replicas: 3
  
  # Load balancing sensitivity
  eventBusSubscriber:
    nats:
      # Consumer group for load balancing
      queue: "sensor-queue"
      # Processing options
      durable: true
      ackWait: 30s
      maxInFlight: 25
```

## 📊 Event Data Templating

### **Template Functions**

#### **String Operations**
```yaml
parameters:
- src:
    value: "{{.Input.github-push.body.ref | substr 11}}"      # Remove prefix
  dest: branch-name
- src:  
    value: "{{.Input.github-push.body.after | substr 0 7}}"   # Short hash
  dest: short-commit
- src:
    value: "{{.Input.github-push.body.repository.name | upper}}"  # Uppercase
  dest: repo-upper
```

#### **JSON Operations**
```yaml
parameters:
- src:
    value: "{{.Input.github-push.body | toJson}}"             # Convert to JSON
  dest: event-json  
- src:
    value: "{{.Input.github-push.body.commits | length}}"     # Array length
  dest: commit-count
```

#### **Conditional Logic**
```yaml
parameters:
- src:
    value: "{{if eq (.Input.github-push.body.ref) \"refs/heads/main\"}}production{{else}}development{{end}}"
  dest: environment
- src:
    value: "{{.Input.github-push.body.head_commit.message | contains \"hotfix:\" | ternary \"high\" \"normal\"}}"
  dest: priority
```

### **Advanced Templating**

#### **Complex Data Extraction**
```yaml
parameters:
# Extract modified YAML files
- src:
    value: |
      {{- $files := list -}}
      {{- range .Input.github-push.body.commits -}}
        {{- range .modified -}}
          {{- if or (hasSuffix . ".yaml") (hasSuffix . ".yml") -}}
            {{- $files = append $files . -}}
          {{- end -}}
        {{- end -}}
      {{- end -}}
      {{- $files | join "," -}}
  dest: modified-yaml-files

# Calculate total file changes
- src:
    value: |
      {{- $total := 0 -}}
      {{- range .Input.github-push.body.commits -}}
        {{- $total = add $total (len .added) -}}
        {{- $total = add $total (len .modified) -}}  
        {{- $total = add $total (len .removed) -}}
      {{- end -}}
      {{- $total -}}
  dest: total-changes
```

## 🚨 Troubleshooting Sensors

### **Dependency Issues**

#### **Check Event Reception**
```bash
# Sensor logs
kubectl logs -n argo-events -l sensor-name=my-sensor -f

# Look for:
# "dependency satisfied"
# "event received" 
# "filter applied"
# "trigger executed"
```

#### **Verify EventSource Reference**
```bash
# Check if EventSource exists
kubectl get eventsource -n argo-events

# Verify event names in EventSource
kubectl get eventsource my-source -o yaml | grep -A 10 webhook

# Check Sensor dependencies match
kubectl get sensor my-sensor -o yaml | grep -A 5 dependencies
```

### **Filter Debugging**

#### **JSON Path Testing**
```bash
# Test JSON path expressions with jq
echo '{"body":{"ref":"refs/heads/main"}}' | jq '.body.ref'

# Test with actual event data  
kubectl logs -n argo-events sensor-pod | grep "event payload" | jq '.body.ref'
```

#### **Expression Testing**
```bash
# Enable debug logging in sensor
kubectl set env deployment/sensor-controller -n argo-events LOG_LEVEL=debug

# Check expression evaluation logs
kubectl logs -n argo-events deployment/sensor-controller | grep expression
```

### **Trigger Execution Issues**

#### **Workflow Trigger Problems**
```bash
# Check Argo Workflows API connectivity
kubectl get workflows -n argo

# Verify RBAC permissions
kubectl auth can-i create workflows --as=system:serviceaccount:argo-events:sensor-sa -n argo

# Check sensor service account
kubectl get sa sensor-sa -n argo-events
kubectl describe rolebinding sensor-binding -n argo-events
```

#### **HTTP Trigger Problems**  
```bash
# Test HTTP endpoint directly
curl -X POST https://api.external.com/webhook \
  -H "Authorization: Bearer token" \
  -d '{"test": "data"}'

# Check network policies
kubectl get networkpolicy -n argo-events

# Check DNS resolution
kubectl exec -n argo-events sensor-pod -- nslookup api.external.com
```

## 🎯 Exam Preparation

### **Critical Sensor Concepts**
1. **Dependencies** - EventSource reference and event naming
2. **Filters** - Data, context, and expression filtering
3. **Triggers** - Workflow, K8s resource, HTTP, Kafka triggers  
4. **Dependency Logic** - AND/OR combinations
5. **Event Data Access** - Template expressions and parameters

### **Common Exam Questions**

#### **"How to trigger workflow only for main branch?"**
```yaml
filters:
  data:
  - path: body.ref
    value: ["refs/heads/main"]
```

#### **"How to combine multiple event dependencies?"**
```yaml
dependencies:
- name: event-a
- name: event-b
dependencyLogic: "event-a && event-b"
```

#### **"How to pass GitHub commit SHA to workflow?"**
```yaml
parameters:
- src:
    dependencyName: github-dep
    dataKey: body.after
  dest: commit-sha
```

#### **"How to execute different workflows for different branches?"**
```yaml
triggers:
- template:
    name: dev-workflow
    conditions: |
      request.body.ref == "refs/heads/develop"
    argoWorkflow: {...}
- template:
    name: prod-workflow  
    conditions: |
      request.body.ref == "refs/heads/main"
    argoWorkflow: {...}
```

### **Configuration Validation Checklist**
- [ ] **EventSource name** matches dependency eventSourceName
- [ ] **Event name** matches EventSource event definition
- [ ] **Filters** use correct JSON paths
- [ ] **Trigger parameters** map event data correctly
- [ ] **RBAC** allows sensor to create target resources
- [ ] **Network access** allows external HTTP calls
- [ ] **Secrets** are referenced correctly in HTTP triggers

## 📚 Next Topics

1. [11 - Event Dependencies](11-event-dependencies.md)
2. [13 - Filters y Conditions](13-filters-conditions.md)  
3. [18 - Integration con Workflows](18-integration-workflows.md)