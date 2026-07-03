# Deploy Argo CD to Kubernetes

### Links
- [ArgoCD Docs](https://argo-cd.readthedocs.io/en/stable/)
- [ArgoCD Github](https://github.com/argoproj/argo-cd)
- [Local Argo Open API Docs](https://localhost:8080/swagger-ui)


Create a namespace for Argo with the following command:

```bash 
$ kubectl create namespace argocd 
```

## Deploy ArgoCD using the quick start manifest with the following command: 

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verify that Argo CD is installed by running the following command: 

```bash
kubectl get pods -n argocd 
```

## Accessing Argo CD 
ArgoCD offers an intuitive and user-friendly web-based interface that simplifies the management of applications deployed on Kubernetes clusters. Below, we will explain access via the UI. Access via the CLI is also possible but explained in detail in Chapter 6. 
Login via UI 
In order to access Argo CD's API, you need to port forward the service. This should be done in a separate terminal window so you can run commands from your current terminal window. 
Command: 

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443 --address 0.0.0.0
```

Argo CD comes with a built-in admin user and auto-generated password. The password is stored as a Kubernetes secret which can be retrieved with the following command in a new terminal (as the port-forward may have taken control of your previous session): 

```bash
$ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kC4f-I9uVxBQuBIF
```

Access the UI on ``` https://localhost:8080 ``` (or your VM’s public IP on port 8080). Due to the self-signed certificate, you will receive a TLS error which you will need to manually approve. 
Pay close attention to the URI. It uses HTTPS and not HTTP. Navigating to ``` http://localhost:8080 ``` will result in a server-side error that breaks the port forwarding. 
On the UI, you can login with the user admin and the auto-generated password. 

Once logged in, you should see the following screen: 

Login via CLI 
Download and install the ArgoCD CLI tool suitable for your operating system and corresponding to your specific ArgoCD release version. More detailed installation instructions can be found in the CLI installation documentation. 
For Ubuntu 24.04, you can install the latest (as of this publication) ArgoCD CLI using the following commands:

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x argocd-linux-amd64
sudo mv argocd-linux-amd64 /usr/local/bin/argocd
```

For users on Mac, Linux, and WSL who prefer using Homebrew, you can install the tool by running the following command: 

```bash
brew install argocd 
```

Using the username admin, the auto-generated password from above, and the port-forwarded service, log in to Argo CD accessible at https://localhost:8080. 
Command: 
$ argocd login localhost:8080 --username admin --password kC4f-I9uVxBQuBIF

Output: 
WARNING: server certificate had error: tls: failed to verify certificate: x509: certificate signed by unknown authority. Proceed insecurely (y/n)? y 
Username: admin 
Password: 
'admin:login' logged in successfully 
Context 'localhost:8080' updated 

Upon successful login, you should see a message indicating that you're logged in to the specified ArgoCD server URL. 

