# 🚀 Deploy do Projeto com Kubernetes + KIND

Este projeto demonstra como criar um cluster **Kubernetes** local utilizando **Kind**, configurar **Ingress NGINX** e implantar uma aplicação composta por **Frontend**, **Backend** e **Banco de Dados PostgreSQL**.

---

## ✅ Pré-requisitos

- [Docker](https://docs.docker.com/get-docker/)
- [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/)

---

## 🔹 Passo 1: Criar o cluster com Kind

Execute o arquivo `kind-config.yaml` com o seguinte conteúdo:

```
kind create cluster --config kind-config.yaml
```
## 🔹 Passo 2: Instalar o Ingress Controller (NGINX)

```
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml

kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```
Isso instalará o Ingress NGINX no namespace ingress-nginx.

## 🔹 Passo 3: Aplicar os manifests da aplicação
Dentro do diretório kube-deploy, aplique os arquivos na ordem:

```
kubectl apply -f namespace.yaml
kubectl apply -f database/
kubectl apply -f backend/
kubectl apply -f frontend/
kubectl apply -f ingress/
```
## 🔹 Passo 4: Configurar DNS local
Edite o arquivo /etc/hosts e adicione:
```
127.0.0.1 app.local
```
## 🔹 Passo 5: Acessar a aplicação

Frontend:
http://app.local/

API Backend:
http://app.local/api/mensagens


## ✅ Estrutura do Projeto

```
projeto-k8s-deploy/
├── namespace.yaml
├── backend/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── configmap.yaml
├── frontend/
│   ├── deployment.yaml
│   ├── service.yaml
├── database/
│   ├── statefulset.yaml
│   ├── pvc.yaml
│   └── secret.yaml
└── ingress/
    └── ingress.yaml
```