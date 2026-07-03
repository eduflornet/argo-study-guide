# 📋 Templates de Workflows

## 🎯 ¿Qué son los Templates?

Los **templates** son la unidad básica de trabajo en Argo Workflows. Cada template define **qué hacer** en un paso específico del workflow. Son **reutilizables** y pueden ser **parametrizados**.

## 🔧 Tipos de Templates

### **1. Container Template**
Ejecuta un contenedor - el tipo más común y básico.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: container-example-
spec:
  entrypoint: container-template
  templates:
  - name: container-template
    container:
      image: alpine:latest
      command: [sh, -c]
      args: ["echo 'Hello from container template'"]
      resources:
        limits:
          memory: "128Mi"
          cpu: "100m"
```

#### **Opciones del Container Template:**
```yaml
- name: advanced-container
  container:
    image: ubuntu:20.04
    command: ["/bin/bash"]
    args: ["-c", "echo 'Processing...' && sleep 5"]
    
    # Variables de entorno
    env:
    - name: MY_VAR
      value: "mi-valor"
    - name: POD_NAME
      valueFrom:
        fieldRef:
          fieldPath: metadata.name
    
    # Trabajar con directorios
    workingDir: /workspace
    
    # Recursos
    resources:
      requests:
        memory: "64Mi"
        cpu: "50m"
      limits:
        memory: "256Mi" 
        cpu: "200m"
        
    # Security context
    securityContext:
      runAsUser: 1000
      runAsNonRoot: true
```

### **2. Script Template** 
Ejecuta scripts inline - ideal para Python, Bash, etc.

```yaml
- name: python-script
  script:
    image: python:3.9
    command: [python]
    source: |
      import json
      import os
      
      # Lógica del script
      data = {
          "timestamp": "2024-01-01",
          "status": "processed",
          "pod": os.environ.get('HOSTNAME', 'unknown')
      }
      
      print(f"Datos procesados: {json.dumps(data)}")
      
      # Guardar resultado
      with open('/tmp/resultado.json', 'w') as f:
          json.dump(data, f)
```

#### **Script con Múltiples Lenguajes:**
```yaml
# Bash script
- name: bash-script
  script:
    image: ubuntu:20.04
    command: [bash]
    source: |
      #!/bin/bash
      echo "Iniciando procesamiento..."
      
      for i in {1..5}; do
        echo "Paso $i de 5"
        sleep 1
      done
      
      echo "Procesamiento completado"
      echo "resultado=exitoso" > /tmp/status.txt

# Node.js script  
- name: node-script
  script:
    image: node:16
    command: [node]
    source: |
      const fs = require('fs');
      
      console.log('Ejecutando script Node.js');
      
      const data = {
        timestamp: new Date().toISOString(),
        nodeVersion: process.version,
        platform: process.platform
      };
      
      fs.writeFileSync('/tmp/data.json', JSON.stringify(data));
      console.log('Datos guardados:', data);
```

### **3. Resource Template**
Crea, actualiza o elimina recursos de Kubernetes.

```yaml
- name: create-configmap
  resource:
    action: create
    manifest: |
      apiVersion: v1
      kind: ConfigMap
      metadata:
        name: workflow-config-{{workflow.uid}}
      data:
        timestamp: "{{workflow.creationTimestamp}}"
        workflow: "{{workflow.name}}"
        
- name: delete-configmap
  resource:
    action: delete
    manifest: |
      apiVersion: v1
      kind: ConfigMap
      metadata:
        name: workflow-config-{{workflow.uid}}
```

#### **Acciones de Resource Template:**
- `create` - Crear recurso
- `apply` - Aplicar cambios
- `delete` - Eliminar recurso  
- `replace` - Reemplazar recurso
- `patch` - Actualizar campos específicos

### **4. Steps Template**
Define una **secuencia** de pasos que se ejecutan secuencial o paralelo.

```yaml
- name: steps-example
  steps:
  # Paso 1: Ejecución paralela
  - - name: step1a
      template: print-message
      arguments:
        parameters:
        - name: message
          value: "Paso 1A"
    - name: step1b
      template: print-message
      arguments:
        parameters:
        - name: message
          value: "Paso 1B"
  
  # Paso 2: Secuencial (después del paso 1)
  - - name: step2
      template: print-message 
      arguments:
        parameters:
        - name: message
          value: "Paso 2 - después de 1A y 1B"
          
  # Paso 3: Otro paso secuencial
  - - name: step3
      template: print-message
      arguments:
        parameters:
        - name: message
          value: "Paso final"
```

### **5. DAG Template**
Define un **Grafo Dirigido Acíclico** con dependencias complejas.

```yaml
- name: dag-example
  dag:
    tasks:
    - name: task-a
      template: print-message
      arguments:
        parameters:
        - name: message
          value: "Tarea A - sin dependencias"
          
    - name: task-b  
      template: print-message
      arguments:
        parameters:
        - name: message
          value: "Tarea B - sin dependencias"
          
    - name: task-c
      template: print-message
      dependencies: [task-a]
      arguments:
        parameters:
        - name: message
          value: "Tarea C - depende de A"
          
    - name: task-d
      template: print-message  
      dependencies: [task-a, task-b]
      arguments:
        parameters:
        - name: message
          value: "Tarea D - depende de A y B"
          
    - name: task-e
      template: print-message
      dependencies: [task-c, task-d]  
      arguments:
        parameters:
        - name: message
          value: "Tarea final - depende de C y D"
```

## 📥📤 Input/Output en Templates

### **Template con Inputs**
```yaml
- name: template-con-inputs
  inputs:
    parameters:
    - name: nombre
      value: "mundo"  # valor por defecto
    - name: veces
      value: "3"
    artifacts:
    - name: archivo-entrada
      path: /tmp/input.txt
      
  container:
    image: alpine:latest
    command: [sh, -c]
    args: |
      for i in $(seq 1 {{inputs.parameters.veces}}); do
        echo "Hola {{inputs.parameters.nombre}} #$i"
      done
      
      # Procesar archivo de entrada si existe
      if [ -f /tmp/input.txt ]; then
        echo "Contenido del archivo:"
        cat /tmp/input.txt
      fi
```

### **Template con Outputs**
```yaml
- name: template-con-outputs
  container:
    image: alpine:latest
    command: [sh, -c]
    args: |
      echo "Generando datos..."
      echo "timestamp=$(date -Iseconds)" > /tmp/resultado.txt
      echo "hostname=$(hostname)" >> /tmp/resultado.txt
      echo "uuid=$(cat /proc/sys/kernel/random/uuid)" >> /tmp/resultado.txt
      
  outputs:
    parameters:
    - name: timestamp
      valueFrom:
        path: /tmp/resultado.txt
    artifacts:
    - name: archivo-resultado
      path: /tmp/resultado.txt
```

## 🔄 Workflow Completo con Templates

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow  
metadata:
  generateName: pipeline-completo-
spec:
  entrypoint: main-pipeline
  
  # Global parameters
  arguments:
    parameters:
    - name: entorno
      value: "desarrollo"
    - name: version
      value: "v1.0.0"
      
  templates:
  # Template principal
  - name: main-pipeline
    dag:
      tasks:
      - name: validacion
        template: validar-entorno
        arguments:
          parameters:
          - name: env
            value: "{{workflow.parameters.entorno}}"
            
      - name: build
        template: build-application
        dependencies: [validacion]
        arguments:
          parameters:
          - name: version
            value: "{{workflow.parameters.version}}"
            
      - name: test-unitarios
        template: run-tests
        dependencies: [build]
        arguments:
          parameters:
          - name: tipo
            value: "unit"
            
      - name: test-integracion  
        template: run-tests
        dependencies: [build]
        arguments:
          parameters:
          - name: tipo
            value: "integration"
            
      - name: deploy
        template: deploy-application
        dependencies: [test-unitarios, test-integracion]
        arguments:
          parameters:
          - name: env
            value: "{{workflow.parameters.entorno}}"
          - name: version
            value: "{{workflow.parameters.version}}"
  
  # Templates reutilizables
  - name: validar-entorno
    inputs:
      parameters:
      - name: env
    script:
      image: alpine:latest
      command: [sh]
      source: |
        echo "Validando entorno: {{inputs.parameters.env}}"
        
        case "{{inputs.parameters.env}}" in
          desarrollo|staging|produccion)
            echo "Entorno válido"
            exit 0
            ;;
          *)
            echo "Entorno inválido"  
            exit 1
            ;;
        esac
        
  - name: build-application
    inputs:
      parameters:  
      - name: version
    container:
      image: docker:latest
      command: [sh, -c]
      args: |
        echo "Building application version {{inputs.parameters.version}}"
        # Simular build  
        sleep 10
        echo "Build completed successfully"
        
  - name: run-tests
    inputs:
      parameters:
      - name: tipo
    script:
      image: python:3.9-alpine
      command: [python]
      source: |
        import time
        import random
        
        test_type = "{{inputs.parameters.tipo}}"
        print(f"Ejecutando tests {test_type}...")
        
        # Simular tests
        time.sleep(5)
        
        # Random success/failure para demo
        if random.random() > 0.1:  # 90% success rate
            print(f"Tests {test_type} passed ✓")
        else:
            print(f"Tests {test_type} failed ✗")
            exit(1)
            
  - name: deploy-application
    inputs:
      parameters:
      - name: env
      - name: version
    resource:
      action: apply
      manifest: |
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: myapp-{{inputs.parameters.env}}
        spec:
          replicas: 2
          selector:
            matchLabels:
              app: myapp
              env: "{{inputs.parameters.env}}"
          template:
            metadata:
              labels:
                app: myapp
                env: "{{inputs.parameters.env}}"
                version: "{{inputs.parameters.version}}"
            spec:
              containers:
              - name: app
                image: myapp:{{inputs.parameters.version}}
                ports:
                - containerPort: 8080
```

## 🎯 Template Best Practices

### **1. Naming Conventions**
```yaml
templates:
- name: build-frontend        # Descriptivo y claro
- name: test-api-integration  # Incluye contexto
- name: deploy-to-k8s        # Acción clara
```

### **2. Parametrización**  
```yaml
# ❌ Hardcoded values
- name: bad-template
  container:
    image: myapp:1.2.3  # Versión fija
    
# ✅ Parameterized
- name: good-template
  inputs:
    parameters:
    - name: image-tag
      value: "latest"
  container:
    image: myapp:{{inputs.parameters.image-tag}}
```

### **3. Resource Limits**
```yaml
# ✅ Siempre especificar recursos
- name: resource-aware-template
  container:
    image: python:3.9
    resources:
      requests:
        memory: "128Mi"
        cpu: "100m"
      limits:
        memory: "512Mi"  
        cpu: "500m"
```

### **4. Error Handling**
```yaml
- name: robust-template
  retryStrategy:
    limit: 3
    backoff:
      duration: "5s"
      factor: 2
  script:
    image: alpine:latest
    command: [sh]
    source: |
      set -e  # Exit on error
      
      echo "Ejecutando operación crítica..."
      
      # Validaciones
      if [ -z "$REQUIRED_VAR" ]; then
        echo "Error: Variable requerida no definida"
        exit 1  
      fi
      
      # Lógica principal
      echo "Operación completada"
```

## 🚨 Errores Comunes en Templates

### **1. Template no encontrado**
```yaml
# ❌ Error: template no existe
spec:
  entrypoint: main-template  # No definido
  templates: []

# ✅ Correcto
spec:
  entrypoint: main-template
  templates:
  - name: main-template
    container:
      image: alpine
```

### **2. Parámetros incorrectos**
```yaml
# ❌ Error: parámetro no definido
- name: template-with-params
  container:
    image: "{{inputs.parameters.nonexistent}}"  # No definido
    
# ✅ Correcto  
- name: template-with-params
  inputs:
    parameters:
    - name: image-name
      value: "alpine"
  container:
    image: "{{inputs.parameters.image-name}}"
```

### **3. Dependencias circulares en DAG**
```yaml
# ❌ Error: dependencia circular
- name: dag-with-cycle
  dag:
    tasks:
    - name: task-a
      template: simple
      dependencies: [task-b]  # A depende de B
    - name: task-b
      template: simple
      dependencies: [task-a]  # B depende de A = CICLO
```

## 📊 Template Patterns

### **Pattern 1: Template Library**
```yaml
# Workflow que reutiliza templates de una librería
apiVersion: argoproj.io/v1alpha1  
kind: WorkflowTemplate
metadata:
  name: common-templates
spec:
  templates:
  - name: notify-slack
    inputs:
      parameters:
      - name: channel
      - name: message
    script:
      image: curlimages/curl
      command: [sh]
      source: |
        curl -X POST "{{inputs.parameters.channel}}" \
          -d '{"text": "{{inputs.parameters.message}}"}'
          
  - name: backup-database
    script:
      image: postgres:13
      command: [sh]
      source: |
        pg_dump $DATABASE_URL > /tmp/backup.sql
        echo "Backup completado"
```

### **Pattern 2: Conditional Templates**
```yaml
- name: conditional-deploy
  steps:
  - - name: check-environment
      template: validate-env
  - - name: dev-deploy
      template: deploy-dev
      when: "{{steps.check-environment.outputs.result}} == dev"
    - name: prod-deploy 
      template: deploy-prod
      when: "{{steps.check-environment.outputs.result}} == prod"
```

## 🎯 Puntos Clave para el Examen

### **Tipos de Templates (MEMORIZAR)**
1. **Container** - Ejecuta contenedores 
2. **Script** - Scripts inline
3. **Resource** - Recursos de K8s
4. **Steps** - Secuencias 
5. **DAG** - Dependencias complejas

### **Template Components**
- **inputs** - Parámetros y artefactos de entrada
- **outputs** - Parámetros y artefactos de salida
- **dependencies** - Solo en DAG templates
- **when** - Ejecución condicional

### **Variables Importantes**
- `{{inputs.parameters.nombre}}`
- `{{outputs.parameters.valor}}`
- `{{workflow.name}}`, `{{workflow.uid}}`
- `{{tasks.task-name.outputs.result}}`

## 📚 Próximos Pasos

Continúa profundizando en:

1. [06 - Especificación de Workflows](06-especificacion-workflows.md)
2. [09 - Manejo de Artefactos](09-manejo-artefactos.md)  
3. [13 - Grafos Dirigidos Acíclicos (DAG)](13-dag-workflows.md)