# Projeto Final — Cluster Docker Swarm na Azure com NFS e Nginx

> **Observação importante**
> Este README preserva **100% das anotações originais do projeto**, apenas corrigindo erros de digitação, comandos quebrados e adicionando explicações técnicas.
> **Todas as máquinas possuem IP público**, conforme implementado no ambiente real.

---

## Visão Geral

Este projeto demonstra a criação de um **cluster Docker Swarm** em **máquinas virtuais na Azure**, utilizando:

* 1 Manager
* 2 Workers
* Rede virtual compartilhada
* IP público em **todas as VMs**
* Replicação de volume via **NFS**
* Balanceamento de carga com **Nginx Proxy**

O objetivo é executar uma aplicação PHP + MySQL de forma distribuída, escalável e testável via stress test.

---

## 1️⃣ Criação das Máquinas Virtuais na Azure

Criar **3 VMs Linux (Ubuntu)**:

* manager-01
* worker-01
* worker-02

### Requisitos obrigatórios

* Todas as VMs **na mesma VNet**
* Podem estar em **sub-redes diferentes** (não é problema)
* **Todas com IP público**
* SSH liberado (porta 22)

### Network Security Group — Regras de Entrada

Criar regras de **ENTRADA** no NSG associado às VMs ou à sub-rede:

| Porta | Protocolo | Origem         | Destino        | Uso               |
| ----- | --------- | -------------- | -------------- | ----------------- |
| 22    | TCP       | Internet       | VM             | SSH               |
| 80    | TCP       | Internet       | VM             | HTTP              |
| 2377  | TCP       | VirtualNetwork | VirtualNetwork | Swarm Manager     |
| 7946  | TCP/UDP   | VirtualNetwork | VirtualNetwork | Comunicação Swarm |
| 4789  | UDP       | VirtualNetwork | VirtualNetwork | Overlay Network   |

---

## 2️⃣ Preparação do Volume Docker (Aplicação)

### Navegação até os volumes

```bash
cd /var/lib/docker/volumes/
ls
cd data/
ls
cd _data/
ls
```

➡️ Aqui está o **banco de dados** utilizado no exemplo da aplicação. Esse banco de dados foi utilizado no módulo de volume, se não tiver criado, será necessário voltar e revisar como criar e subir.

```bash
cd ..
cd ..
ls
cd app
ls
cd _data/
```

### Editando a aplicação

```bash
nano index.php
```

* Inserir o código do `index.php` (arquivo do GitHub)
* **Alterar o IP do Manager (IP público da Azure)**
* Ajustar usuário e senha do banco levantado no container

---

## 3️⃣ Subindo o Container Web Manualmente (Teste Inicial)

```bash
docker run --name web-server -dt \
-p 80:80 \
--mount type=volume,src=app,dst=/app \
webdevops/php-apache:alpine-php7
```

Esse passo valida:

* Volume
* Aplicação
* Porta 80

---

## 4️⃣ Stress Test da Aplicação

### Criar arquivo do Loader.io

* Acessar o site do Loader.io
* Obter o arquivo de verificação

```bash
cd /var/lib/docker/volumes/app/_data
nano loaderio-verificacao.txt
```

* Colar o conteúdo do arquivo
* Criar e executar o teste pelo site

---

## 5️⃣ Inicializando o Docker Swarm

### No Manager

```bash
docker ps
docker rm --force web-server
docker swarm init
```

* Copiar o comando `docker swarm join`
* Executar **nos dois workers**
* Usar **IP público do Manager**

### Criando o serviço replicado

```bash
docker service create \
--name web-server \
--replicas 3 \
-p 80:80 \
--mount type=volume,src=app,dst=/app \
webdevops/php-apache:alpine-php7
```

```bash
docker service ps web-server
```

---

## 6️⃣ Replicação de Volume com NFS

### No Servidor (Manager)

```bash
apt-get update
apt-get install -y nfs-kernel-server
```

```bash
cd /var/lib/docker/volumes/app/_data
nano /etc/exports
```

Conteúdo:

```
/var/lib/docker/volumes/app/_data *(rw,sync,no_subtree_check)
```

```bash
exportfs -ar
showmount -e
```

### Nos Workers

```bash
apt-get install -y nfs-common
```

```bash
mount -t nfs <IP_PUBLICO_MANAGER>:/var/lib/docker/volumes/app/_data /var/lib/docker/volumes/app/_data
```

➡️ Repetir o processo nos **dois workers**.

---

## 7️⃣ Criando Proxy Reverso com Nginx

```bash
mkdir /proxy
cd /proxy
nano nginx.conf
```

* Copiar o arquivo do GitHub
* Alterar os **IPs públicos das VMs**

```bash
nano Dockerfile
```

* Copiar o Dockerfile do GitHub

### Build e execução

```bash
docker build -t proxy-app .
docker container run --name my-proxy-app -d -p 4500:4500 proxy-app
```

---

## 8️⃣ Stress Test Final

* Acessar novamente o Loader.io
* Executar o teste contra o **IP público do proxy**
* Validar distribuição de carga e estabilidade

---

## Resultado Final

* Cluster Swarm funcional
* Volume compartilhado via NFS
* Balanceamento via Nginx
* Aplicação escalável
* Ambiente validado por stress test

---

## Observações Técnicas

* Sub-redes diferentes **não impactam** o Swarm se estiverem na mesma VNet
* IP público em todas as VMs facilita debug e SSH
* Em produção, IP público seria apenas no proxy

---

🚀 Projeto finalizado com sucesso.
