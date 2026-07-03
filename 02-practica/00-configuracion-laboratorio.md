# 🧪 Laboratorio 01: Configuración del Entorno

> **🎯 Objetivos**: 
> - Configurar un cluster Kubernetes local
> - Instalar Argo CD
> - Crear tu primera Application
> - Familiarizarse con la UI y CLI

## ⏱️ Tiempo Estimado: 60-90 minutos

## 📋 Prerequisitos
- Docker instalado y funcionando
- Linux/macOS/WSL2 (Windows)
- 4GB RAM mínimo
- Conexión a internet estable

---

## 🔧 Paso 1: Configurar Cluster Local

### **Opción A: Kind (Recomendado)**

#### **1.1 Instalar Kind**
```bash
# Linux/macOS
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Verificar instalación
kind version
```

#### **1.2 Crear Cluster**
```bash
# Crear cluster básico
kind create cluster --name argocd-lab

# Verificar cluster
kubectl cluster-info --context kind-argocd-lab
kubectl get nodes
```

### **Opción B: Minikube**
```bash
# Instalar minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Crear cluster
minikube start --driver=docker --memory=4096 --cpus=2

# Verificar
kubectl get nodes
```

### **✅ Checkpoint 1:**
```bash
# Deberías ver algo así:
kubectl get nodes
# NAME                 STATUS   ROLES           AGE   VERSION
# argocd-lab-control-plane   Ready    control-plane   2m    v1.27.3
```

---

## 🚀 Paso 2: Instalar Argo CD

### **2.1 Instalar ArgoCD CLI**
```bash
# Descargar latest version
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64

# Instalar
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64

# Verificar
argocd version --client
```

### **2.2 Instalar ArgoCD en el Cluster**
```bash
# Crear namespace
kubectl create namespace argocd

# Instalar ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Esperar a que todos los pods estén ready
kubectl wait --for=condition=Ready pods --all -n argocd --timeout=600s
```

### **2.3 Verificar Instalación**
```bash
# Ver todos los pods de ArgoCD
kubectl get pods -n argocd

# Deberías ver algo como:
# NAME                                                READY   STATUS    RESTARTS   AGE
# argocd-application-controller-0                     1/1     Running   0          2m
# argocd-applicationset-controller-xxx                1/1     Running   0          2m  
# argocd-dex-server-xxx                               1/1     Running   0          2m
# argocd-notifications-controller-xxx                 1/1     Running   0          2m
# argocd-redis-xxx                                    1/1     Running   0          2m
# argocd-repo-server-xxx                              1/1     Running   0          2m
# argocd-server-xxx                                   1/1     Running   0          2m
```

### **✅ Checkpoint 2:**
```bash
# Todos los pods deben estar Running y Ready 1/1
kubectl get pods -n argocd | grep -v Running
# (No debería mostrar nada)
```

---

## 🌐 Paso 3: Acceder a la UI de Argo CD

### **3.1 Obtener Password Inicial**
```bash
# Obtener password inicial del admin
argocd_password=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
echo "ArgoCD Admin Password: $argocd_password"

# Guardarlo para uso posterior
echo $argocd_password > ~/argocd-password.txt
```

### **3.2 Hacer Port Forward**
```bash
# Port forward al ArgoCD server (en terminal separado)
kubectl port-forward svc/argocd-server -n argocd 8080:443

# O ejecutar en background
kubectl port-forward svc/argocd-server -n argocd 8080:443 &
```

### **3.3 Acceder a la UI**
```bash
# Abrir en browser
open http://localhost:8080
# O navegar a: https://localhost:8080

# Login credentials:
# Username: admin  
# Password: (el que obtuviste en 3.1)
```

### **💡 Tip:** Si usas HTTPS, acepta el certificado self-signed en tu browser.

### **✅ Checkpoint 3:**
- [ ] UI de ArgoCD carga correctamente
- [ ] Puedes hacer login con admin
- [ ] Ves el dashboard principal (vacío inicialmente)

---

## 🔐 Paso 4: Configurar CLI de Argo CD

### **4.1 Login via CLI**
```bash
# Login desde CLI (en terminal nuevo)
argocd login localhost:8080

# Usar estas credenciales:
# Username: admin
# Password: (el que guardaste)
# WARNING: acepta certificado inseguro con 'y'
```

### **4.2 Verificar Conexión**
```bash
# Verificar que CLI está conectado
argocd app list

# Debería mostrar:
# NAME  CLUSTER  NAMESPACE  PROJECT  STATUS  HEALTH  SYNCPOLICY  CONDITIONS  REPO  PATH  TARGET
# (vacío inicialmente)
```

### **4.3 Ver Información del Sistema**
```bash
# Ver información del cluster
argocd cluster list

# Ver información del usuario actual  
argocd account list

# Ver configuración actual
argocd context
```

### **✅ Checkpoint 4:**
- [ ] CLI conectado exitosamente
- [ ] `argocd app list` funciona sin errores
- [ ] Puedes ver cluster information

---

## 📦 Paso 5: Crear Repositorio de Ejemplo

### **5.1 Fork del Repo de Ejemplos**
```bash
# Crear directorio de trabajo
mkdir -p ~/argocd-labs
cd ~/argocd-labs

# Clonar repo de ejemplos oficial
git clone https://github.com/argoproj/argocd-example-apps.git
cd argocd-example-apps

# Ver estructura
ls -la
# Deberías ver directorios como: guestbook, helm-guestbook, kustomize-guestbook, etc.
```

### **5.2 Explorar Aplicación de Ejemplo**
```bash
# Ver la aplicación guestbook  
ls -la guestbook/
cat guestbook/guestbook-ui-deployment.yaml

# Es un deployment estándar de Kubernetes
```

### **✅ Checkpoint 5:**
- [ ] Repo clonado exitosamente
- [ ] Puedes ver contenido de `guestbook/`
- [ ] Entiendes que son manifests YAML normales de K8s

---

## 🎯 Paso 6: Crear Primera Application

### **6.1 Crear Application via CLI**
```bash
# Desde directorio argocd-example-apps
argocd app create guestbook \
  --repo https://github.com/argoproj/argocd-example-apps.git \
  --path guestbook \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default \
  --sync-policy automated \
  --auto-prune \
  --self-heal
```

### **6.2 Verificar Application**
```bash
# Ver la nueva application
argocd app list

# Ver detalles completos
argocd app get guestbook

# Ver status de sync
argocd app sync guestbook
```

### **6.3 Verificar en UI**
1. Refresh la UI en http://localhost:8080
2. Deberías ver la aplicación "guestbook"
3. Click en ella para ver detalles
4. Explora los recursos creados

### **✅ Checkpoint 6:**
- [ ] Application "guestbook" aparece en CLI
- [ ] Application visible en UI
- [ ] Status shows "Synced" y "Healthy"

---

## 🔍 Paso 7: Explorar la Application

### **7.1 Ver Recursos en Kubernetes**
```bash
# Ver pods creados por la application
kubectl get pods -l app=guestbook

# Ver servicios
kubectl get svc -l app=guestbook  

# Ver deployments
kubectl get deployments -l app=guestbook
```

### **7.2 Acceder a la Aplicación**
```bash
# Port forward al guestbook
kubectl port-forward svc/guestbook-ui 9090:80

# Acceder en browser
open http://localhost:9090
# O navegar a: http://localhost:9090
```

### **7.3 Explorar en UI de ArgoCD**
En la UI de ArgoCD:
1. Click en "guestbook" application
2. Explora el graph view
3. Click en cada resource (Deployment, Service, etc.)
4. Ve las relationships entre recursos

### **✅ Checkpoint 7:**
- [ ] Pods de guestbook running
- [ ] Aplicación accesible en http://localhost:9090  
- [ ] UI de ArgoCD muestra resources correctamente

---

## 🔄 Paso 8: Probar GitOps Flow

### **8.1 Hacer un Cambio en Git**
```bash
# Crear fork personal o usar directorio local
cd ~/argocd-labs
git clone https://github.com/argoproj/argocd-example-apps.git my-guestbook
cd my-guestbook

# Editar replicas del deployment
sed -i 's/replicas: 1/replicas: 3/g' guestbook/guestbook-ui-deployment.yaml

# Ver el cambio
git diff

# Comitir cambio (si tienes tu propio fork)
git add .
git commit -m "Scale guestbook to 3 replicas"
```

### **8.2 Crear Application con tu Fork**
```bash
# (Solo si tienes tu propio fork en GitHub)
argocd app create my-guestbook \
  --repo https://github.com/TU_USUARIO/argocd-example-apps.git \
  --path guestbook \
  --dest-namespace default \
  --dest-server https://kubernetes.default.svc \
  --sync-policy automated
```

### **8.3 Ver Sincronización Automática**
```bash
# Monitorear la sincronización
watch argocd app get my-guestbook

# Or ver en UI - debería detectar cambio automáticamente
```

### **✅ Checkpoint 8:**
- [ ] Cambio detectado por ArgoCD
- [ ] Application se sincroniza automáticamente  
- [ ] 3 replicas del pod corriendo

---

## 🧪 Paso 9: Experimentos Adicionales

### **9.1 Probar Drift Detection**
```bash
# Hacer cambio manual en cluster (esto es "drift")  
kubectl scale deployment guestbook-ui --replicas=5

# Ver status en ArgoCD
argocd app get guestbook

# Con self-heal habilitado, debería revertir automáticamente a Git state
```

### **9.2 Probar Manual Sync**
```bash
# Crear app sin auto-sync
argocd app create manual-guestbook \
  --repo https://github.com/argoproj/argocd-example-apps.git \
  --path kustomize-guestbook \
  --dest-namespace manual \
  --dest-server https://kubernetes.default.svc

# Crear namespace primero
kubectl create namespace manual

# Manual sync required
argocd app sync manual-guestbook
```

### **9.3 Explorar Different App Types**
```bash
# Helm app
argocd app create helm-guestbook \
  --repo https://github.com/argoproj/argocd-example-apps.git \
  --path helm-guestbook \
  --dest-namespace helm \
  --dest-server https://kubernetes.default.svc

kubectl create namespace helm
argocd app sync helm-guestbook
```

---

## 🎓 Paso 10: Validación Final

### **10.1 Checklist de Comandos Críticos**
```bash
# Estos comandos DEBES dominar para el examen
argocd app list
argocd app get <app-name>  
argocd app sync <app-name>
argocd app diff <app-name>
argocd app delete <app-name>

kubectl get applications -n argocd
kubectl describe application <app-name> -n argocd

# Test todos estos comandos
```

### **10.2 UI Navigation Test**
Navega la UI y encuentra:
- [ ] Applications list
- [ ] Application details
- [ ] Sync status y history
- [ ] Resource tree view
- [ ] Logs de cada resource
- [ ] Settings y configuration

### **10.3 Troubleshooting Practice**
```bash
# Simular problemas y resolverlos
# 1. Delete un pod manualmente, ver como ArgoCD lo detecta
kubectl delete pod -l app=guestbook

# 2. Ver logs de ArgoCD components  
kubectl logs -n argocd deployment/argocd-application-controller

# 3. Verificar repository connection
argocd repo list
```

---

## 📝 Resumen del Laboratorio

### **🎯 Lo que Lograste:**
- ✅ Configuraste cluster Kubernetes local
- ✅ Instalaste Argo CD exitosamente  
- ✅ Creaste y configuraste tu primera Application
- ✅ Exploraste GitOps workflow básico
- ✅ Probaste UI y CLI de ArgoCD
- ✅ Experimentaste con drift detection y self-heal

### **🔧 Comandos Key que Practicaste:**
```bash
# Cluster management
kind create cluster
kubectl get nodes

# ArgoCD installation  
kubectl apply -f argocd-install.yaml
kubectl get pods -n argocd

# Application management
argocd app create
argocd app get  
argocd app sync
argocd app list

# Kubernetes native
kubectl get applications
kubectl describe application
```

### **🧠 Conceptos que Aprendiste:**
- **Declarative GitOps**: Manifests en Git → ArgoCD sync → Kubernetes
- **Pull Model**: ArgoCD pulls changes from Git, no Push from CI
- **Self-Heal**: Automatic correction of manual drift
- **Sync Policies**: Manual vs Automated sync behavior

---

## 🚀 Próximos Pasos

### **🔬 Laboratorio Recomendado:**
- **Lab 02**: [Configuración Avanzada de Applications](lab-02-applications-avanzadas.md)
- **Lab 03**: [Helm y Kustomize Integration](lab-03-helm-kustomize.md)
- **Lab 04**: [RBAC y Multi-tenant Setup](lab-04-rbac-multienant.md)

### **📚 Teoría de Refuerzo:**
- Revisar: [Arquitectura ArgoCD](../01-teoria/02-argo-cd/01-arquitectura-argocd.md)
- Estudiar: [Applications y Projects](../01-teoria/02-argo-cd/03-applications-projects.md)

### **💡 Para el Examen:**
- **Memorizar** los comandos que usaste hoy
- **Practicar** creación de Applications hasta hacerlo sin referencia
- **Entender** completamente el flujo GitOps que experimentaste

---

## 🚨 Troubleshooting Común

### **Problema: Pods no empiezan**
```bash
# Check system resources
kubectl top nodes
kubectl describe nodes

# Check pod events
kubectl get events --sort-by='.lastTimestamp'
```

### **Problema: ArgoCD UI no carga**
```bash
# Verify port-forward
kubectl get svc -n argocd
netstat -tulpn | grep 8080

# Check ArgoCD server logs  
kubectl logs -n argocd deployment/argocd-server
```

### **Problema: Application OutOfSync**
```bash
# Check repository access
argocd repo get <repo-url>

# Check application details
argocd app get <app-name>
argocd app diff <app-name>
```

---

¡Felicitaciones! Has completado tu primer laboratorio de ArgoCD. 🎉

**Next Challenge**: Trata de crear 2-3 applications más usando diferentes repositorios y paths. ¡La práctica hace al maestro!