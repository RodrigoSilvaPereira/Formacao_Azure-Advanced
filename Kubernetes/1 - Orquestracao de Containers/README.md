# ☸️ Módulo: Orquestração de Contêineres com Kubernetes (K8s)

O **Kubernetes**, carinhosamente chamado de **K8s**, é uma plataforma de código aberto (originalmente desenvolvida pelo Google) projetada para automatizar a implantação, o dimensionamento e o gerenciamento de aplicações em contêineres.

---

## ❓ Por que precisamos do Kubernetes?

À medida que as aplicações evoluem de **Monólitos** (um único bloco de código) para **Microsserviços**, a complexidade de gerenciar centenas ou milhares de contêineres manualmente torna-se inviável.

O K8s resolve as seguintes dores:

- **Disponibilidade Absoluta**: Diminuição drástica de downtime.  
- **Escalabilidade Elástica**: Aumento e diminuição de performance conforme a demanda.  
- **Recuperação de Desastres**: Funções nativas de backup e restore.  
- **Flexibilidade de Ambiente**: Roda em infraestrutura local, VMs, Cloud Pública ou Híbrida.  

---

## 🏗️ Arquitetura e Conceitos Fundamentais

Para entender o Kubernetes, precisamos visualizar a hierarquia do ambiente.

### 1️⃣ O Cluster

É o conjunto total de máquinas (nós) que trabalham juntas.  
Pense no cluster como o **"datacenter virtual"** onde tudo acontece.

Ele é composto por:

- **Plano de Controle (Control Plane)**  
  O cérebro do cluster. Decide onde os Pods vão rodar, monitora o estado do sistema e responde a eventos.

- **Nós (Nodes)**  
  Máquinas físicas ou virtuais que realmente executam as aplicações.

---

### 2️⃣ O Nó (Node)

Cada nó possui os serviços necessários para rodar contêineres:

- **Kubelet** → Agente que se comunica com o Control Plane  
- **Container Runtime** → Docker, containerd, etc  

---

### 3️⃣ O Pod (A Menor Unidade)

O **Pod** é a unidade atômica do Kubernetes.

- **Abstração**: Você não gerencia o contêiner diretamente; você gerencia o Pod.  
- **Composição**: Um Pod pode conter um ou mais contêineres que compartilham o mesmo endereço IP e armazenamento.  
- **Regra de Ouro**: Geralmente executamos uma aplicação por Pod.  

---

## 🔄 Ciclo de Vida do Gerenciamento

O fluxo padrão de trabalho no Kubernetes segue seis passos essenciais:

1. **Criar o Cluster**  
   Configurar o plano de controle e os nós.

2. **Implantar (Deploy)**  
   Subir sua aplicação no cluster.

3. **Explorar**  
   Verificar logs, status e saúde dos Pods.

4. **Expor**  
   Criar um Service para tornar a aplicação acessível (interna ou externamente).

5. **Escalar**  
   Aumentar o número de réplicas para suportar o tráfego.

6. **Atualizar**  
   Realizar o Rolling Update da aplicação sem interromper o serviço.

---

## 📝 Nota Técnica

O nome **"K8s"** vem da contagem das 8 letras entre o "K" e o "s" na palavra **Kubernetes**.

---

### 📚 Anotações de Aula  
**Formação Azure Advanced — Módulo Kubernetes**
