# 🔄 Argo Workflows

## 🎯 Objetivos de Aprendizaje CAPA (36% del examen)

Este módulo cubre la porción más importante del examen CAPA. Al completarlo deberás:

- ✅ **Understand Argo Workflow Fundamentals** - Comprender los fundamentos 
- ✅ **Generating and Consuming Artifacts** - Generar y consumir artefactos
- ✅ **Understand Argo Workflow Templates** - Entender las plantillas de workflow
- ✅ **Understand the Argo Workflow Spec** - Comprender la especificación de workflows
- ✅ **Work with DAG (Directed-Acyclic Graphs)** - Trabajar con grafos dirigidos acíclicos
- ✅ **Run Data Processing Jobs** - Ejecutar trabajos de procesamiento de datos

## 📚 Contenidos del Módulo

### 1. Fundamentos
- [01 - Introducción a Argo Workflows](01-introduccion-workflows.md)
- [02 - Arquitectura y Componentes](02-arquitectura-componentes.md)
- [03 - Instalación y Configuración](03-instalacion-configuracion.md)
- [04 - Conceptos Clave](04-conceptos-clave.md)

### 2. Templates y Especificaciones
- [05 - Templates de Workflows](05-templates-workflows.md)
- [06 - Especificación de Workflows](06-especificacion-workflows.md)  
- [07 - Tipos de Templates](07-tipos-templates.md)
- [08 - Parámetros y Variables](08-parametros-variables.md)

### 3. Artefactos y Datos
- [09 - Manejo de Artefactos](09-manejo-artefactos.md)
- [10 - Generación de Artefactos](10-generacion-artefactos.md)
- [11 - Consumo de Artefactos](11-consumo-artefactos.md)
- [12 - Almacenamiento y Persistencia](12-almacenamiento-persistencia.md)

### 4. DAGs y Flujos Complejos
- [13 - Grafos Dirigidos Acíclicos (DAG)](13-dag-workflows.md)
- [14 - Dependencias y Paralelismo](14-dependencias-paralelismo.md)
- [15 - Control de Flujo](15-control-flujo.md)
- [16 - Patrones de Workflows](16-patrones-workflows.md)

### 5. Casos de Uso Avanzados
- [17 - Procesamiento de Datos](17-procesamiento-datos.md)
- [18 - Pipelines de ML](18-pipelines-ml.md)
- [19 - CI/CD con Workflows](19-cicd-workflows.md)
- [20 - Workflows Condicionales](20-workflows-condicionales.md)

### 6. Operaciones y Monitoreo
- [21 - Gestión de Workflows](21-gestion-workflows.md)
- [22 - Monitoreo y Logging](22-monitoreo-logging.md)
- [23 - Debugging y Troubleshooting](23-debugging-troubleshooting.md)
- [24 - Best Practices](24-best-practices.md)

## 🎯 Puntos Clave para el Examen

### Fundamentos Esenciales (MEMORIZAR)
- **Workflow**: Definición de una secuencia de pasos de trabajo
- **Template**: Plantilla reutilizable que define qué hacer en cada paso
- **Step**: Un paso individual dentro de un workflow  
- **DAG**: Grafo dirigido acíclico que define dependencias complejas
- **Artifact**: Archivos o datos que pasan entre pasos

### Templates Principales
- **Container**: Ejecuta un contenedor
- **Script**: Ejecuta un script inline
- **Resource**: Crea/modifica recursos de Kubernetes 
- **DAG**: Define un grafo de dependencias
- **Steps**: Define una secuencia de pasos paralelos/secuenciales

### Artefactos Críticos
- **Input Artifacts**: Datos que entran a un template
- **Output Artifacts**: Datos que salen de un template  
- **Artifact Repository**: Almacén de artefactos (S3, GCS, etc.)
- **Path**: Ubicación donde se montan los artefactos

### DAG Concepts
- **Dependencies**: Relaciones entre tareas
- **Tasks**: Unidades de trabajo en un DAG
- **Parallelism**: Ejecución simultánea de tareas
- **Retry**: Política de reintentos

## 📋 Lab Exercises

Para aprobar el examen deberás poder realizar estas tareas prácticas:

1. **Crear Workflows Básicos**
   - Workflow simple con container template
   - Workflow con script template
   - Workflow con parámetros

2. **Manejo de Artefactos**
   - Generar artefactos desde un contenedor
   - Pasar artefactos entre pasos
   - Configurar repositorio de artefactos

3. **Workflows DAG**
   - Crear DAG con dependencias múltiples
   - Implementar paralelismo controlado
   - Manejar fallos y reintentos

4. **Data Processing**
   - Pipeline de procesamiento de datos
   - Workflow con múltiples stages
   - Integración con almacenamiento externo

## 🚨 Errores Comunes en el Examen

- ❌ Confundir `steps` con `dag` templates
- ❌ Mal uso de `dependencies` en DAG
- ❌ Incorrecta especificación de artifact paths  
- ❌ No especificar `entrypoint` en el workflow
- ❌ Usar nombres incorrectos para parámetros/artefactos

## 📖 Recursos de Referencia

- [Documentación Oficial Argo Workflows](https://argoproj.github.io/argo-workflows/)
- [Argo Workflows Examples](https://github.com/argoproj/argo-workflows/tree/master/examples)
- [API Reference](https://argoproj.github.io/argo-workflows/fields/)

## ✅ Checklist de Preparación

Para estar listo para el examen, asegúrate de poder:

- [ ] Explicar qué es un Workflow y sus componentes principales
- [ ] Crear workflows usando diferentes tipos de templates  
- [ ] Configurar y usar artefactos para pasar datos
- [ ] Diseñar DAGs complejos con múltiples dependencias
- [ ] Implementar workflows para procesamiento de datos
- [ ] Debuggear workflows que fallan
- [ ] Aplicar best practices de seguridad y performance