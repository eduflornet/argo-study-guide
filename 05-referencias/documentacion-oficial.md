# 📚 Referencias y Recursos - CAPA

> **🎯 Objetivo**: Compilación completa de recursos oficiales y recomendados para CAPA

## 🌟 Recursos Oficiales (CRÍTICOS)

### **📖 Documentación Principal**
| Recurso | URL | Descripción |
|---------|-----|-------------|
| **CAPA Official** | [argo-project.org/certification](https://argo-project.org/certification) | Página oficial del examen |
| **Argo CD Docs** | [argo-cd.readthedocs.io](https://argo-cd.readthedocs.io/) | Documentación completa |
| **Argo Workflows** | [argoproj.github.io/argo-workflows](https://argoproj.github.io/argo-workflows) | Workflows documentation |
| **Argo Events** | [argoproj.github.io/argo-events](https://argoproj.github.io/argo-events) | Events documentation |
| **Argo Rollouts** | [argo-rollouts.readthedocs.io](https://argo-rollouts.readthedocs.io/) | Rollouts documentation |

### **🐙 Repositorios GitHub**
```bash
# Clona estos repos para explorar ejemplos
git clone https://github.com/argoproj/argo-cd.git
git clone https://github.com/argoproj/argo-workflows.git  
git clone https://github.com/argoproj/argo-events.git
git clone https://github.com/argoproj/argo-rollouts.git
git clone https://github.com/argoproj/argo-cd-example-apps.git  # ⭐ EJEMPLOS
```

---

## 📺 Cursos y Formación

### **🎓 Cursos Recomendados**

#### **1. Cloud Native Computing Foundation (CNCF)**
- **GitOps Fundamentals** - Gratuito
- **Kubernetes Fundamentals** - Prerequisito recomendado
- **URL**: [training.linuxfoundation.org](https://training.linuxfoundation.org)

#### **2. Udemy Courses**
- **"Argo CD for GitOps"** by Rahul Wagh ⭐
- **"Complete GitOps with ArgoCD"** by TechWorld with Nana
- **"Kubernetes GitOps with ArgoCD"** by Mumshad Mannambeth

#### **3. A Cloud Guru**  
- **"GitOps with ArgoCD"** - Hands-on labs
- **"Kubernetes Deep Dive"** - Complementario

#### **4. Pluralsight**
- **"Getting Started with ArgoCD"** by Roland Guijt
- **"Kubernetes CI/CD with GitOps"** by Dan Llewellyn

### **💡 Formación Gratuita**
```yaml
Free Resources:
  - Argo Project YouTube Channel: ⭐⭐⭐⭐⭐
  - KubeCon GitOps talks: ⭐⭐⭐⭐
  - CNCF Webinars: ⭐⭐⭐
  - RedHat OpenShift GitOps: ⭐⭐⭐
```

---

## 📖 Libros y Artículos

### **📚 Libros Recomendados**

#### **1. "GitOps and Kubernetes" by Jesse Suen, Billy Yuen**
```yaml
Rating: ⭐⭐⭐⭐⭐
Focus: ArgoCD específico
Pages: 350+
Best for: Exam preparation
```

#### **2. "Learning GitOps" by Steve Buchanan**
```yaml
Rating: ⭐⭐⭐⭐
Focus: GitOps general + ArgoCD  
Pages: 200+
Best for: Conceptual understanding
```

#### **3. "Kubernetes Up & Running" by Kelsey Hightower**
```yaml
Rating: ⭐⭐⭐⭐⭐
Focus: Kubernetes fundamentals
Best for: Prerequisites
```

### **📄 Artículos Clave**

#### **GitOps Fundamentals:**
- [GitOps: What you need to know](https://www.gitops.tech/) - Weaveworks
- [Guide to GitOps](https://www.weave.works/technologies/gitops/) - Official GitOps Guide
- [GitOps Best Practices](https://codefresh.io/learn/gitops/gitops-best-practices/)

#### **ArgoCD Specific:**
- [ArgoCD Best Practices](https://argo-cd.readthedocs.io/en/stable/user-guide/best_practices/)
- [Scaling ArgoCD](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#scaling)
- [ArgoCD Security](https://argo-cd.readthedocs.io/en/stable/operator-manual/security/)

---

## 🎥 Contenido de Video

### **📹 YouTube Channels**

#### **1. ArgoProj Channel** ⭐⭐⭐⭐⭐
```yaml
URL: youtube.com/c/argoproject
Content:
  - Official tutorials
  - Community meetings
  - New features demos
  - KubeCon presentations
```

#### **2. TechWorld with Nana** ⭐⭐⭐⭐⭐
```yaml
ArgoCD Playlist:
  - "Complete ArgoCD Tutorial"
  - "GitOps with ArgoCD" 
  - "ArgoCD vs FluxCD"
```

#### **3. Just me and Opensource** ⭐⭐⭐⭐
```yaml
Focus: Hands-on tutorials
Best Videos:
  - "ArgoCD from Zero to Hero"
  - "ArgoCD Multi-cluster Setup"
```

### **🎬 Webinars Importantes**
| Título | Organización | Duración | Link |
|--------|--------------|----------|------|
| GitOps Days | CNCF | 8h+ | [gitopsdays.com](https://www.gitopsdays.com) |
| ArgoCD Office Hours | Argo Project | 1h weekly | [argo-project.org](https://argo-project.org) |
| GitOps with ArgoCD | RedHat | 2h | [redhat.com/webinars](https://redhat.com/webinars) |

---

## 💻 Laboratorios y Práctica

### **🔬 Plataformas de Labs**

#### **1. Katacoda (Killercoda)** ⭐⭐⭐⭐⭐
```yaml
URL: killercoda.com/argocd
Scenarios:
  - ArgoCD Installation
  - First Application
  - Multi-cluster Setup
Cost: FREE
```

#### **2. Play with Kubernetes** ⭐⭐⭐⭐
```yaml
URL: labs.play-with-k8s.com  
Benefits:
  - Free K8s clusters
  - 4 hours sessions
  - Perfect for ArgoCD testing
```

#### **3. Instruqt** ⭐⭐⭐
```yaml
ArgoCD Tracks: Available
Cost: Some free, some paid
Quality: High
```

### **🏠 Local Labs Setup**

#### **Option 1: Kind (Kubernetes in Docker)** ⭐⭐⭐⭐⭐
```bash
# Install Kind
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Create cluster
kind create cluster --config=kind-config.yaml
```

#### **Option 2: Minikube** ⭐⭐⭐⭐
```bash
# Install minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start cluster
minikube start --driver=docker
```

#### **Option 3: k3s (Raspberry Pi)** ⭐⭐⭐
```bash
# Lightweight K8s
curl -sfL https://get.k3s.io | sh -
```

---

## 🌐 Comunidades y Soporte

### **💬 Slack Channels**

#### **1. CNCF Slack** ⭐⭐⭐⭐⭐
```yaml
Join: slack.cncf.io
Channels:
  - #argo-cd: Principal channel
  - #argo-workflows: Workflows discussion  
  - #argo-events: Events specific
  - #argo-rollouts: Rollouts discussion
  - #argoproj-dev: Development discussions
```

#### **2. Kubernetes Slack** ⭐⭐⭐⭐
```yaml
Join: kubernetes.slack.com
Channels:
  - #gitops: General GitOps discussion
  - #sig-apps: Application deployment
```

### **📞 Discord Servers**
- **CNCF Community Discord** - General cloud native discussion
- **DevOps Community** - Broader than just Kubernetes

### **🐦 Twitter/X**
```yaml
Follow These Accounts:
  - @argoproj: Official Argo Project
  - @weaveworks: GitOps pioneers  
  - @kubernetesio: K8s updates
  - @CloudNativeFdn: CNCF news
  - @TechWorldWithNana: Educational content
```

### **📺 Reddit Communities**
- **r/kubernetes** - General K8s discussion
- **r/devops** - DevOps practices including GitOps
- **r/gitops** - GitOps specific discussions

---

## 🛠️ Herramientas Complementarias

### **🧰 CLI Tools Esenciales**

#### **Instalación Completa:**
```bash
# ArgoCD CLI
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd

# Argo Workflows CLI
curl -sLO https://github.com/argoproj/argo-workflows/releases/download/v3.5.0/argo-linux-amd64.gz
gunzip argo-linux-amd64.gz
chmod +x argo-linux-amd64
sudo mv ./argo-linux-amd64 /usr/local/bin/argo

# Kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Helm
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm

# Kustomize
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | bash
sudo mv kustomize /usr/local/bin/
```

### **🔧 Useful Plugins**

#### **kubectl plugins:**
```bash
# kubectl-argo-rollouts plugin
curl -LO https://github.com/argoproj/argo-rollouts/releases/latest/download/kubectl-argo-rollouts-linux-amd64
chmod +x ./kubectl-argo-rollouts-linux-amd64
sudo mv ./kubectl-argo-rollouts-linux-amd64 /usr/local/bin/kubectl-argo-rollouts

# kubectx (context switching)
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```

---

## 📊 Simuladores de Examen

### **🎯 Mock Exams**

#### **1. Killer Shell** ⭐⭐⭐⭐⭐
```yaml
URL: killer.sh
Coverage: CAPA-style questions
Cost: €36 for 2 sessions
Quality: High, similar to real exam
```

#### **2. Udemy Practice Tests** ⭐⭐⭐⭐
```yaml  
Search: "CAPA Practice Test"
Multiple options available
Cost: $10-50
Reviews: Check carefully
```

#### **3. WhizLabs** ⭐⭐⭐
```yaml
ArgoCD Practice Tests: Available
Cost: Monthly subscription
Quality: Good for knowledge check  
```

### **🔄 Custom Practice**
```bash
# Create your own questions using:
# 1. Generate random scenarios
# 2. Official docs examples
# 3. GitHub issue discussions
# 4. Community troubleshooting posts
```

---

## 📱 Apps y Extensiones

### **🔌 VSCode Extensions**
- **Kubernetes** by Microsoft - YAML support
- **YAML** by Red Hat - Better YAML editing
- **GitLens** - Git integration
- **Helm Intellisense** - Helm chart support

### **📱 Mobile Apps**
- **Anki** - Flashcards for command memorization
- **Notion/Obsidian** - Note organization
- **Forest** - Focus and time management during study

---

## 🎯 Tips para Usar las Referencias

### **📅 Study Schedule Integration:**
```yaml
Week 1-2: Focus on official docs + basic videos
Week 3-4: Add community resources + advanced articles  
Week 5-6: Practice labs + hands-on tutorials
Week 7-8: Mock exams + troubleshooting communities
```

### **🔍 How to Research:**
1. **Start with Official Docs** - Always primary source
2. **Cross-reference** with community articles 
3. **Validate** in hands-on labs
4. **Test understanding** with practice questions

### **⚠️ Recursos a Evitar:**
- **Outdated content** (>2 years old)
- **Non-official exam prep** without reviews
- **Brain dumps** (against ethics and ineffective)
- **Purely theoretical** content without hands-on

---

## 📈 Tracking Your Learning

### **📚 Learning Journal Template:**
```markdown
## Daily Learning Log
**Date**: ___________
**Hours**: ___ theory + ___ hands-on  
**Topics Covered**:
- [ ] Topic 1
- [ ] Topic 2

**Commands Practiced**:
- `command 1`
- `command 2`

**Questions/Doubts**:
1. ________________
2. ________________

**Next Day Focus**:
- [ ] Action 1
- [ ] Action 2
```

---

## 🏆 Success Stories

### **🎉 Community Success Tips:**
> **"Practice commands daily - muscle memory is key"** - CAPA graduate
> 
> **"Focus 60% on ArgoCD, it's the bulk of the exam"** - Recent passer
>
> **"Join Slack early, the community is incredibly helpful"** - Study group member

### **📊 Common Success Patterns:**
- **150+ hours** total study time on average
- **Daily practice** beats weekend cramming  
- **Hands-on labs** are more valuable than theory-only
- **Community engagement** accelerates learning

---

¡Con estos recursos tienes todo lo necesario para aprobar CAPA! 🚀

**Recuerda**: Quality over quantity - mejor dominar los recursos oficiales que dispersarse en demasiadas fuentes.