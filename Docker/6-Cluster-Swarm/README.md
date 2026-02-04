# 📂 Módulo: Docker Swarm - Clusterização e Alta Disponibilidade

O **Docker Swarm** transforma um grupo de máquinas Docker em um único motor virtual. Ele é nativo, o que significa que se você tem o Docker instalado, você já tem o Swarm.

---

## 🏗️ 1. Topologia do Laboratório na Azure

Para este laboratório, utilizaremos **3 Máquinas Virtuais** com Ubuntu Server na Azure:

1.  **Manager-01** (IP Interno: `10.0.0.4`) - O "Cérebro" do cluster.
2.  **Worker-01** (IP Interno: `10.0.0.5`) - Executa os containers.
3.  **Worker-02** (IP Interno: `10.0.0.6`) - Executa os containers.

### 🔐 Requisito Azure: Abertura de Portas (NSG)
Portas necessárias no Azure Firewall/NSG:
* **2377/TCP:** Gerenciamento do Cluster.
* **7946/TCP e UDP:** Descoberta de nós.
* **4789/UDP:** Rede Overlay.

---

## 🚀 2. Inicializando o Cluster (No Manager-01)

```bash
sudo docker swarm init --advertise-addr 10.0.0.4
```

---

## 🛠️ 3. Adicionando os Workers ao Cluster

Em cada VM Worker, cole o token gerado:

```bash
sudo docker swarm join --token [SEU_TOKEN] 10.0.0.4:2377
```

Verifique no Manager:
```bash
sudo docker node ls
```

---

## 📦 4. Gerenciando Serviços

```bash
# Criar serviço com 3 réplicas
sudo docker service create --name meu-web -p 80:80 --replicas 3 nginx

# Escalar para 10 réplicas
sudo docker service scale meu-web=10

# Remover serviço
sudo docker service rm meu-web
```

---

## 📜 5. Docker Stack (Orquestração Completa)

```bash
# Criar a stack
sudo docker stack deploy -c docker-stack.yml minha-app

# Listar stacks
sudo docker stack ls
```

---
*Notas de aula: Azure Advanced - Módulo Docker Swarm.*
