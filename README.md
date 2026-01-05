# 🐳 Configuração do Docker & Cluster Kubernetes (Minikube)

Projeto acadêmico desenvolvido para demonstrar a instalação e configuração do **Docker Desktop** e de um **Cluster Kubernetes local com Minikube**, incluindo a execução de containers, criação de deployments e services.

---

## 📖 Sumário
- [Objetivo](#-objetivo)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Instalação do WSL](#-instalação-do-wsl)
- [Instalação do Docker Desktop](#-instalação-do-docker-desktop)
- [Execução de Container Nginx](#-execução-de-container-nginx)
- [Instalação do Minikube](#-instalação-do-minikube)
- [Criação do Cluster Kubernetes](#-criação-do-cluster-kubernetes)
- [Deployment YAML](#-deployment-yaml)
- [Service YAML](#-service-yaml)
- [Resultado Final](#-resultado-final)
- [Autor](#-autor)

---

## 🎯 Objetivo
Demonstrar na prática como configurar o Docker Desktop e o Kubernetes (via Minikube) em ambiente Windows, utilizando **WSL 2** como subsistema Linux.  
O projeto mostra:
- Instalação e configuração do Docker Desktop.
- Criação e execução de containers.
- Configuração de um cluster Kubernetes local.
- Deploy de múltiplos pods com Nginx.
- Exposição de serviços via LoadBalancer.

---

## 🛠 Tecnologias Utilizadas
- **Windows 10 Pro**
- **WSL 2 (Ubuntu)**
- **Docker Desktop 4.52.0**
- **Minikube v1.37.0**
- **Kubectl**
- **Nginx (imagem oficial do Docker Hub)**

---

## ⚙️ Instalação do WSL
```powershell
wsl --install

Instalação do Ubuntu como distribuição padrão.

Após reiniciar, o terminal abre automaticamente para concluir a configuração.

🐳 Instalação do Docker Desktop
Download da versão para Windows (AMD64).

Configuração recomendada:

✅ Usar WSL 2 em vez de Hyper-V.

✅ Atalho na área de trabalho.

🌐 Execução de Container Nginx
Baixar a imagem:

powershell
docker pull nginx
Rodar o container:

powershell
docker run --name nginx-do-ramon -d -p 8080:80 nginx
Acessar no navegador:

Código
http://localhost:8080
☸️ Instalação do Minikube
Download do instalador .exe para Windows.

Instalação padrão em C:\Program Files\Kubernetes\Minikube.

📦 Criação do Cluster Kubernetes
Inicializar cluster com 3 nós:

powershell
minikube start --nodes 3
kubectl get nodes
Resultado esperado:

Código
NAME           STATUS   ROLES           AGE   VERSION
minikube       Ready    control-plane   35m   v1.34.0
minikube-m02   Ready    <none>          32m   v1.34.0
minikube-m03   Ready    <none>          30m   v1.34.0
📄 Deployment YAML
Arquivo: arq-deploy-ramon.yaml

yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
Aplicar no cluster:

powershell
kubectl apply -f arq-deploy-ramon.yaml
kubectl get pods
🔗 Service YAML
Arquivo: arq-service-ramon.yaml

yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
Aplicar no cluster:

powershell
kubectl apply -f arq-service-ramon.yaml
kubectl get service nginx-service
🌍 Resultado Final
Para expor o serviço:

powershell
minikube tunnel
Acessar no navegador:

Código
http://localhost:8090
Página exibida:

Código
Welcome to nginx!
If you see this page, the nginx web server is successfully installed and working.

👨‍🎓 Autor
Ramon Queiroz e Silva

---
