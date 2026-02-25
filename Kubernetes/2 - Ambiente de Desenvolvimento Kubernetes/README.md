# 🧪 Módulo: Ambiente de Desenvolvimento Kubernetes

Para estudar e testar Kubernetes localmente sem depender de infraestrutura em cloud, utilizamos ferramentas que simulam um cluster real em ambiente controlado.

---

## 📌 Minikube

O **Minikube** é um utilitário que permite executar o Kubernetes localmente.

Ele cria um **cluster de nó único** dentro de uma máquina virtual (ou container), permitindo estudar e testar o Kubernetes sem precisar configurar um cluster completo em produção.

### 🎯 Por que usar Minikube?

- Ambiente isolado para aprendizado
- Não exige infraestrutura cloud
- Ideal para testes locais de Deployments, Services e Ingress
- Permite simular cenários reais de cluster

---

## 🧰 Pré-requisitos do Minikube

Antes da instalação, sua máquina deve atender aos seguintes requisitos:

- Software de virtualização instalado (VirtualBox, Hyper-V, etc.)
- 2 CPUs ou mais
- 2 GB de RAM (mínimo)
- 20 GB de espaço em disco
- Conexão com a internet

> ⚠️ Recomendação prática: Utilize 4GB+ de RAM para melhor performance.

---

## ⚙️ Instalação do Minikube (Windows)

### 1️⃣ Baixar o instalador

Acesse:  
https://minikube.sigs.k8s.io/docs/start/

Selecione o sistema operacional compatível e faça o download.

### 2️⃣ Instalar

Execute o instalador normalmente.

---

## 🚀 Iniciando o Cluster

Após a instalação:

1. Abra o **PowerShell**
2. Execute:

```bash
minikube start
```

O Minikube irá:

- Criar a máquina virtual automaticamente
- Configurar o cluster
- Inicializar o Kubernetes

---

### ⏹️ Para parar o cluster

```bash
minikube stop
```

---

## 🔧 Kubectl

O **kubectl** é a ferramenta de linha de comando do Kubernetes.

Ele permite:

- Implantar aplicações
- Inspecionar recursos do cluster
- Gerenciar Pods, Deployments e Services
- Visualizar logs
- Executar comandos dentro de containers

---

## 📥 Instalação do Kubectl

### 1️⃣ Acesse:

https://kubernetes.io/docs/tasks/tools/

Escolha o sistema operacional e siga as instruções.

---

### 2️⃣ Configuração no Windows

- Baixe o arquivo `.exe`
- Crie uma pasta `C:\kubectl`
- Coloque o executável dentro dessa pasta
- Abra Variáveis de Ambiente do Sistema
- Adicione `C:\kubectl` ao PATH

---

### ✅ Testar instalação

```bash
kubectl version --client
```

Se o comando responder corretamente, está configurado.

---

## 🧠 Boas Práticas

Verificar contexto atual:

```bash
kubectl config current-context
```

Verificar nós do cluster:

```bash
kubectl get nodes
```

Confirmar status do Minikube:

```bash
minikube status
```

---

## 🚀 Aplicação no Mundo Real

Embora o Minikube seja apenas para desenvolvimento, os conceitos aplicados aqui são idênticos aos usados em:

- AKS (Azure Kubernetes Service)
- EKS (AWS)
- GKE (Google Cloud)

Aprender localmente significa estar preparado para ambientes produtivos.

---

## 📝 Notas Técnicas

- Minikube cria um cluster de nó único (Single Node Cluster).
- Ideal para aprendizado e testes.
- Não substitui ambiente de produção.

---

### 📚 Anotações de Aula  
Formação Azure Advanced — Módulo Kubernetes
