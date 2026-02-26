# ☁️ Módulo: Cluster Kubernetes em Nuvem

Este módulo aborda a estrutura de um cluster Kubernetes em produção e como criá-lo nos principais provedores de nuvem: AWS, Azure e GCP.

---

# 🏗️ Cluster Kubernetes em Produção

## 📌 Estrutura Geral

Ao implantar o Kubernetes, você cria um **cluster**.

Um cluster Kubernetes consiste em um conjunto de servidores chamados **Nodes (Nós)**, responsáveis por executar aplicações em contêineres.

Todo cluster possui pelo menos:

- Um **Worker Node**
- Uma **Camada de Gerenciamento (Control Plane)**

---

# 🧠 Componentes do Kubernetes

## 🔹 Worker Node

O Worker:

- Hospeda os **Pods**
- Executa as aplicações
- Possui kubelet e runtime de contêiner

Em produção, normalmente há múltiplos nós para garantir:

- Alta disponibilidade
- Escalabilidade
- Tolerância a falhas

---

## 🔹 Camada de Gerenciamento (Control Plane)

Responsável por:

- Tomar decisões globais
- Agendar Pods
- Manter estado desejado
- Responder a eventos

Em produção, roda em múltiplas máquinas para alta disponibilidade.

---

# ⚙️ Componentes do Control Plane

## ● kube-apiserver

Valida e processa requisições REST.

É o **frontend do cluster**.  
Todos os componentes se comunicam através dele.

---

## ● etcd

Banco de dados chave-valor distribuído.

- Armazena o estado do cluster
- Base de dados principal do Kubernetes
- Extremamente crítico

Sem etcd, o cluster não existe.

---

## ● kube-scheduler

Responsável por:

- Escolher qual Node executará cada Pod
- Avaliar recursos disponíveis
- Respeitar restrições

Ele **não executa**, apenas decide onde executar.

---

## ● kube-controller-manager

Executa loops de controle.

Ele compara:

Estado atual → Estado desejado  
E toma ações para corrigir diferenças.

---

# 🔧 Administração do Cluster

## ● kubeadm
Ferramenta para criar clusters manualmente.

## ● kubelet
Executa em todos os Nodes e gerencia Pods localmente.

## ● kubectl
CLI para interação com o cluster.

---

# ☁️ Amazon EKS

O **Amazon Elastic Kubernetes Service (EKS)** é um serviço gerenciado.

Ele elimina a necessidade de administrar o Control Plane.

Você gerencia apenas os Worker Nodes.

---

## 🔹 Serviços equivalentes

- Google Kubernetes Engine (GKE)
- Azure Kubernetes Service (AKS)

---

# 🚀 Criando Cluster Kubernetes na AWS (EKS)

---

## 1️⃣ Instalar AWS CLI

Baixar conforme seu sistema operacional.

Verificar instalação:

```bash
aws --version
```

---

## 2️⃣ Criar credenciais IAM

No console AWS:

- IAM
- My Security Credentials
- Access Keys
- Create Access Key
- Download Key File

---

## 3️⃣ Configurar AWS CLI

```bash
aws configure
```

Preencher:

- Access Key ID
- Secret Access Key
- Região (ex: sa-east-1)
- Output format (opcional)

---

## 4️⃣ Criar Cluster via Console

EKS → Clusters → Create

Configurar:

- Nome
- Versão
- Service Role
- VPC / Subnets
- Security Groups
- Endpoint Public

Create.

⚠️ Atenção à região.

---

## 5️⃣ Verificar Status via CLI

```bash
aws eks --region sa-east-1 describe-cluster --name Kubernetes-lab --query cluster.status
```

---

## 6️⃣ Atualizar kubeconfig

\`\`\`bash
aws eks --region sa-east-1 update-kubeconfig --name Kubernetes-lab
\`\`\`

Se houver erro por conflito com Minikube, remova o arquivo:

~/.kube/config

---

## 7️⃣ Testar conexão

```bash
kubectl get svc
```

---

# 🖥️ Criando Node Group (Workers)

Dentro do EKS:

- Aba Compute
- Add Node Group

Permissões EC2 necessárias:

- AmazonEKS_CNI_Policy
- AmazonEKSWorkerNodePolicy
- AmazonEC2ContainerRegistryReadOnly

Escolher:

- Tipo de instância
- Mínimo e máximo de nós

Create.

---

## Verificar Nodes

```bash
kubectl get nodes --watch
```

---

## Encerrar Cluster

1. Excluir Node Groups
2. Excluir Cluster

---

# ☁️ Azure Kubernetes Service (AKS)

---

## 1️⃣ Instalar Azure CLI

```bash
az version
```

Login:

```bash
az login
```

---

## 2️⃣ Criar Resource Group

```bash
az group create --name rg-k8s-lab --location brazilsouth
```

---

## 3️⃣ Criar Cluster AKS

```bash
az aks create \
--resource-group rg-k8s-lab \
--name aks-lab \
--node-count 2 \
--enable-addons monitoring \
--generate-ssh-keys
```

---

## 4️⃣ Conectar ao Cluster

```bash
az aks get-credentials --resource-group rg-k8s-lab --name aks-lab
```

Testar:

```bash
kubectl get nodes
```

---

# ☁️ Google Kubernetes Engine (GKE)

---

## 1️⃣ Instalar Google Cloud SDK

No PowerShell:

```bash
Install-Module GoogleCloud
Set-ExecutionPolicy RemoteSigned
```

Instalar plugin:

```bash
gcloud components install gke-gcloud-auth-plugin
```

Login:

```bash
gcloud auth login
```

---

## 2️⃣ Criar Cluster GKE

```bash
gcloud container clusters create gke-lab \
--num-nodes=2 \
--region=southamerica-east1
```

---

## 3️⃣ Conectar ao Cluster

```bash
gcloud container clusters get-credentials gke-lab --region southamerica-east1
```

Testar:

```bash
kubectl get nodes
```

---

# 🧠 Conclusão Técnica

Em produção real:

- Você não gerencia Control Plane manualmente
- Usa serviço gerenciado (EKS, AKS, GKE)
- Foca em Nodes, workloads e segurança
- Usa infraestrutura como código (Terraform, Bicep, etc.)

---

# 📚 Anotações de Aula  
Formação Azure Advanced — Módulo Kubernetes
