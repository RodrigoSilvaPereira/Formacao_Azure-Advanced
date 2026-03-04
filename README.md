# ☁️ Formação Azure Advanced

Este repositório consolida minha evolução prática em **Arquitetura Cloud Native na Azure**, estruturada em três pilares fundamentais da engenharia moderna:

- 🐳 Containerização com Docker  
- ☸️ Orquestração com Kubernetes  
- ⚡ Arquitetura Serverless com Azure Functions  

A trilha foi construída de forma progressiva, saindo da infraestrutura básica até arquiteturas distribuídas, escaláveis e orientadas a eventos.

---

## 🗺️ Estrutura do Repositório

| Módulo | Foco | Descrição |
| :--- | :--- | :--- |
| [**🐳 Docker**](./Docker/) | Containerização | Criação, otimização e clusterização de containers com foco em performance, persistência e alta disponibilidade. |
| [**☸️ Kubernetes**](./Kubernetes/) | Orquestração | Deploy, escalabilidade, persistência e CI/CD em cluster distribuído (GKE). |
| [**⚡ Serverless**](./Serverless/) | Arquitetura Event-Driven | APIs com Azure Functions, API Management e integração com banco de dados em modelo pay-per-use. |

---

## 🐳 Docker — Base da Modernização

Evolução de aplicações tradicionais para ambientes containerizados.

**Consolidado:**
- Docker Engine em Linux
- Bind Mounts e Named Volumes
- Controle de CPU/RAM
- Multi-stage builds (<20MB)
- Docker Compose
- Cluster Docker Swarm na Azure
- Alta disponibilidade com NFS + Nginx

Transição arquitetural:

    Aplicação Local → Container → Cluster Swarm

---

## ☸️ Kubernetes — Orquestração em Escala

Evolução da containerização para orquestração completa em ambiente distribuído.

**Consolidado:**
- Pods, ReplicaSets e Deployments
- Services (ClusterIP, NodePort, LoadBalancer)
- PersistentVolume e PersistentVolumeClaim
- StorageClass e NFS
- Deploy em GKE
- Pipeline CI/CD com atualização automática

Transição arquitetural:

    Container → Cluster → Cloud → Deploy Automatizado

---

## ⚡ Serverless — Arquitetura Orientada a Eventos

Evolução do backend tradicional para modelo totalmente gerenciado pela nuvem.

**Consolidado:**
- Fundamentos do modelo Serverless
- Azure Functions (HTTP Triggers)
- CRUD completo com MongoDB
- API Management (governança e segurança)
- Escalabilidade automática (pay-per-use)
- Arquitetura desacoplada por função

Transição arquitetural:

    Backend Monolítico → Funções Independentes → API Gerenciada → Cloud Native

---

## 🚀 Competências Consolidadas

- Arquitetura Cloud Native
- Microsserviços
- Containers e Orquestração
- Serverless Computing
- Persistência em Ambientes Distribuídos
- CI/CD
- Infraestrutura como Código
- Escalabilidade Horizontal
- Alta Disponibilidade
- Design de Sistemas Distribuídos

---

## 📈 Evolução Técnica Completa

Este repositório representa a consolidação da jornada:

    Infraestrutura → Containerização → Orquestração → Serverless

Com foco real em arquitetura moderna, escalável e pronta para ambientes produtivos.

---

*Este repositório compõe meu portfólio de especialização em Azure Advanced e consolida minha evolução prática em Engenharia Cloud.*