# Módulo Docker Swarm: Clusterização e Alta Disponibilidade

![Docker Swarm](https://img.shields.io/badge/Docker-Swarm-2496ED)
![Microsoft Azure](https://img.shields.io/badge/Microsoft_Azure-0089D6)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420)

Este módulo aborda o **Docker Swarm**, o orquestrador nativo do Docker, focado em **clusterização, alta disponibilidade e resiliência** de aplicações. O Swarm permite gerenciar múltiplos nós (VMs) como se fossem um único sistema lógico, simplificando deploy, escalonamento e manutenção.

---

## Índice

- [Infraestrutura na Azure](#infraestrutura-na-azure)
- [Inventário de Máquinas](#inventário-de-máquinas)
- [Configuração de Rede](#configuração-de-rede)
- [Inicializando o Cluster](#inicializando-o-cluster)
- [Gestão de Serviços](#gestão-de-serviços)
- [Persistência de Dados](#persistência-de-dados)
- [Redes Overlay](#redes-overlay)
- [Atualizações e Rollback](#atualizações-e-rollback)
- [Manutenção do Cluster](#manutenção-do-cluster)
- [Notas de Aula](#notas-de-aula)

---

## Infraestrutura na Azure

### Cenário Real

O laboratório utiliza **3 Máquinas Virtuais com Ubuntu Server**, todas no mesmo:

- **Resource Group**
- **Virtual Network (VNet)**

Esse cenário garante baixa latência, comunicação privada e aderência a boas práticas de arquitetura em cloud.

---

## Inventário de Máquinas

| Nome da VM   | Função   | IP Privado (Exemplo) | Descrição |
|-------------|----------|---------------------|-----------|
| manager-01  | Manager  | 10.0.0.4            | Controla o cluster e mantém o estado dos serviços |
| worker-01   | Worker   | 10.0.0.5            | Executa containers (tasks) |
| worker-02   | Worker   | 10.0.0.6            | Executa containers (tasks) |

---

## Configuração de Rede

### Requisito Azure — Network Security Group (NSG)

Para o funcionamento correto do Swarm, as seguintes portas devem estar **liberadas entre os nós**:

| Porta / Protocolo | Finalidade |
|------------------|------------|
| 2377/TCP | Comunicação de gerenciamento do cluster |
| 7946/TCP e UDP | Descoberta de nós e controle |
| 4789/UDP | Tráfego da rede Overlay (containers) |

**Boas práticas**:
- Origem: `VirtualNetwork`
- Evite abrir essas portas para a Internet pública

---

## Inicializando o Cluster

### Passo 1 — Inicializar o Manager

No **manager-01**:

```bash
sudo docker swarm init --advertise-addr 10.0.0.4
```

Esse comando retorna um **token de join**, necessário para adicionar workers.

---

### Passo 2 — Adicionar Workers

No **worker-01** e **worker-02**:

```bash
sudo docker swarm join --token SWMTKN-1-XYZ... 10.0.0.4:2377
```

---

### Passo 3 — Validar o Cluster

No **manager-01**:

```bash
sudo docker node ls
```

Saída esperada:

```text
ID        HOSTNAME    STATUS  AVAILABILITY  MANAGER STATUS  ENGINE VERSION
xyz...    manager-01  Ready   Active        Leader          20.10.7
abc...    worker-01   Ready   Active                        20.10.7
def...    worker-02   Ready   Active                        20.10.7
```

---

## Gestão de Serviços

No Docker Swarm, trabalhamos com o conceito de **Service**, não containers isolados.

### Modo Replicated (padrão)

```bash
sudo docker service create \
  --name web-nginx \
  --replicas 3 \
  -p 80:80 \
  nginx
```

O Swarm distribui automaticamente as réplicas entre os nós disponíveis.

---

### Modo Global

Executa exatamente **uma instância por nó**:

```bash
sudo docker service create \
  --name monitor \
  --mode global \
  prom/node-exporter
```

Uso comum: agentes de monitoramento e logging.

---

### Escalonamento (Scaling)

```bash
sudo docker service scale web-nginx=10
```

Escala horizontal em segundos, sem downtime.

---

### Comandos Úteis

```bash
sudo docker service ls
sudo docker service ps web-nginx
sudo docker service rm web-nginx
```

---

## Persistência de Dados

### O Problema

Containers são **efêmeros**. Em um cluster, uma task pode mudar de nó e perder dados locais.

### A Solução

Utilizar **armazenamento centralizado**, como:
- Azure Files (NFS)
- Servidor NFS dedicado

---

### Configuração do NFS (em todos os nós)

```bash
sudo apt-get update
sudo apt-get install nfs-common -y
sudo mkdir -p /mnt/dados_compartilhados
sudo mount -t nfs ENDERECO_AZURE:/nome_share /mnt/dados_compartilhados
```

---

### Serviço com Persistência Real

```bash
sudo docker service create \
  --name app-web \
  --replicas 3 \
  --mount type=bind,src=/mnt/dados_compartilhados,dst=/usr/share/nginx/html \
  nginx
```

---

## Redes Overlay

Permitem comunicação **segura entre containers em hosts diferentes**.

```bash
sudo docker network create --driver overlay rede-infra
sudo docker network ls
```

Aplicando a rede a um serviço:

```bash
sudo docker service create \
  --name backend \
  --network rede-infra \
  --replicas 2 \
  minha-api
```

---

## Atualizações e Rollback

### Rolling Update

```bash
sudo docker service update \
  --image nginx:1.21 \
  --update-delay 10s \
  web-nginx
```

---

### Política de Atualização na Criação

```bash
sudo docker service create \
  --name meu-app \
  --replicas 5 \
  --update-delay 10s \
  --update-parallelism 2 \
  --update-failure-action rollback \
  nginx:1.20
```

---

### Rollback

```bash
sudo docker service update --rollback web-nginx
sudo docker service ps web-nginx
```

---

## Manutenção do Cluster

### Drenar um Nó

```bash
sudo docker node update --availability drain worker-01
```

### Reativar o Nó

```bash
sudo docker node update --availability active worker-01
```

---

### Promoção e Rebaixamento

```bash
sudo docker node promote worker-01
sudo docker node demote manager-02
```

---

### Remoção de Nó

```bash
sudo docker node update --availability drain worker-01
sudo docker swarm leave
sudo docker node rm worker-01
```

---

## Notas de Aula

- Alta disponibilidade nativa
- Load Balancing interno via DNS
- Comunicação criptografada por padrão
- Curva de aprendizado menor que Kubernetes

**Formação:** Azure Advanced — Orquestração de Clusters
