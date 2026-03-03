# ☸️ Formação Azure Advanced: Módulo Kubernetes

Este diretório centraliza todo o aprendizado sobre **Kubernetes**, consolidando a evolução da containerização com Docker para a orquestração completa de aplicações em ambientes distribuídos, tanto locais quanto em nuvem (GCP).

O conteúdo foi estruturado de forma progressiva, partindo dos fundamentos da orquestração até arquiteturas com persistência de dados e deploy em cluster cloud.

---

## 🗺️ Mapa do Repositório

| Módulo | Descrição | Status |
| :--- | :--- | :---: |
| [**1. Orquestração de Containers**](./1%20-%20Orquestracao%20de%20Containers/) | Conceitos fundamentais de orquestração e introdução ao Kubernetes. | ✅ |
| [**2. Ambiente de Desenvolvimento Kubernetes**](./2%20-%20Ambiente%20de%20Desenvolvimento%20Kubernetes/) | Instalação, Minikube, kubectl e estrutura inicial de cluster local. | ✅ |
| [**3. Cluster Kubernetes em Nuvem**](./3%20-%20Cluster%20Kubernetes%20em%20Nuvem/) | Criação e gerenciamento de cluster no GCP (GKE). | ✅ |
| [**4. Pods em Kubernetes**](./4%20-%20Pods%20em%20Kubernetes/) | Estrutura de Pods, YAML, Deployments e escalabilidade. | ✅ |
| [**5. Serviços de Acesso para Pods**](./5%20-%20Servicos%20de%20Acesso%20para%20Pods/) | NodePort, LoadBalancer, Port Forward e execução interna em containers. | ✅ |
| [**6. Persistência de Dados em Clusters Kubernetes**](./6%20-%20Persistencia%20de%20Dados%20em%20Clusters%20Kubernetes/) | PV, PVC, StorageClass, NFS e persistência em ambiente cloud. | ✅ |
| [**7. Projeto Final**](./7%20-%20Projeto%20Final/) | Pipeline CI/CD com Docker e Deploy automatizado no GKE. | ✅ |

---

## 🚀 Tecnologias e Conceitos Dominados

* **Orquestração de Containers:** Pods, ReplicaSets e Deployments.
* **Exposição de Aplicações:** Services (ClusterIP, NodePort e LoadBalancer).
* **Escalabilidade:** Escala horizontal manual e arquitetural.
* **Persistência:** PersistentVolume (PV), PersistentVolumeClaim (PVC) e NFS.
* **Cloud Native:** Deploy em Google Kubernetes Engine (GKE).
* **CI/CD:** Pipeline automatizado com build de imagem e atualização contínua no cluster.
* **Arquitetura Distribuída:** Separação entre aplicação, banco de dados e camada de rede.

---

## 🏆 Projeto Final

> **Resultado:** Pipeline CI/CD com deploy automatizado de aplicação Node.js em cluster Kubernetes no GCP.

- Build automático de imagem Docker
- Push para registry
- Atualização contínua no Deployment
- Exposição pública via LoadBalancer
- Escalabilidade horizontal

---

## 📈 Evolução Técnica Consolidada

Este módulo representa a transição completa de:

```
Container Local → Cluster → Cloud → Deploy Automatizado
```

Consolidando conhecimentos em:

- Docker
- Kubernetes
- GCP
- Infraestrutura como Código
- CI/CD
- Arquitetura Cloud Native

---

*Este repositório é parte do meu portfólio de especialização em Azure Advanced.*