# 📘 Curso LFS256 - DevOps and Workflow Management with Argo

Notas y contenido del curso oficial **LFS256** de la Linux Foundation.

## 📄 Fichero de notas

[LFS256-curso](/07-curso-lfs256/LFS256-curso.es.md) — Notas completas del curso en español.
[CAPA curriculum](/07-curso-lfs256/CAPA_curriculum.md) — Temario oficial del examen CAPA.

---

## 🗂️ Índice de contenidos

### Conceptos previos
| Sección | Descripción |
|---|---|
| [Canary Deployment](#) | Estrategia de despliegue gradual por porcentaje de usuarios |
| [Blue-Green Deployment](#) | Dos entornos paralelos con switch de tráfico |

### Capítulo 9-15 · Introducción a Argo
| Sección en notas | Tema |
|---|---|
| `# 9.Introduction to Argo` | Visión general del ecosistema Argo |
| `# 10.Essential Concepts for Argo` | GitOps: principios, declarativo, inmutable, automatización |
| `# 11.Argo Overview` | Argo CD, Workflows, Events, Rollouts |
| `# 12.Benefits of Using Argo` | Ventajas y casos de uso |
| `# 13.Step-by-Step: Deploying Kubernetes for Argo` | Kubernetes como base de Argo |
| `# 14.Installing Kubernetes Locally` | Docker, kubectl, kind — Ubuntu y Fedora |
| `# 15.Take this quiz...` | Quiz capítulo 2 con respuestas |

### Capítulo 16-24 · Argo CD *(40% del examen)*
| Sección en notas | Tema |
|---|---|
| `# 16.Chapter Overview and Objectives` | Objetivos del capítulo |
| `# 17.The Architecture of Argo CD` | Vocabulario: target state, live state, sync, refresh |
| `# 18.Core Components` | API Server, Repository Server, Application Controller |
| `# 19.Reconciliation and Synchronization Control` | Reconciliation loop, resource hooks, sync waves |
| `# 20.Objects & Resources` | Application CRD, AppProject, credenciales |
| `# 21.Argo CD Extensions & Integrations` | Plugins, Notifications, Image Updater |
| `# 22.Best Practices` | RBAC, secrets, actualizaciones, Helm/Kustomize |
| `# 23.Lab Exercises` | Labs 3.1, 3.2, 3.3 |
| `# 24.Questions` | Quiz capítulo 3 con respuestas comentadas |

### Capítulo 25-27 · Argo Workflows *(30% del examen)*
| Sección en notas | Tema |
|---|---|
| `# 25.Argo Workflows` | Estructura de un Workflow, tipos de templates |
| `# 26.Defining Argo Workflows and Its Components` | Argo Server, Workflow Controller, Argo UI |
| `# 27.Questions_Workflows` | Quiz capítulo 4 con respuestas comentadas |

### Capítulo 28-31 · Argo Rollouts *(15% del examen)*
| Sección en notas | Tema |
|---|---|
| `# 28.Argo Rollouts` | CI/CD, Progressive Delivery, estrategias de despliegue |
| `# 29.Building Blocks of Argo Rollouts` | Controller, Rollout Resource, Ingress, Service, AnalysisTemplate |
| `# 30.Argo Rollouts Architecture and Core Components` | Rollout vs Deployment, migración, workloadRef |
| `# 31.Argo Rollouts Questions` | Quiz capítulo 5 con respuestas comentadas |

### Capítulo 32-33 · Argo Events *(15% del examen)*
| Sección en notas | Tema |
|---|---|
| `# 32.Argo Events` | Event Source, Sensor, EventBus, Trigger |
| `# 33.Argo Events Questions` | Quiz capítulo 6 con respuestas comentadas |

### Capítulo 35 · Preguntas finales
| Sección en notas | Tema |
|---|---|
| `# 35.Final Questions` | 10 preguntas de repaso global con respuestas |

---

## 🎯 Cobertura del examen CAPA

| Herramienta | % Examen | Capítulos cubiertos |
|---|---|---|
| Argo CD | 40% | 16–24 |
| Argo Workflows | 30% | 25–27 |
| Argo Events | 15% | 32–33 |
| Argo Rollouts | 15% | 28–31 |
