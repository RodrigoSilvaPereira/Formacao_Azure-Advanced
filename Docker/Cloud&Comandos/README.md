# 🐳 Azure Advanced: Fundamentos de Docker e Cloud

Este repositório faz parte da trilha de especialização em **Azure Advanced**, documentando a transição da infraestrutura tradicional para o ecossistema de containers e orquestração.

---

# 📂 Módulo: Cloud & Comandos Principais

Este diretório contém a base fundamental do Docker, cobrindo desde a transição de conceitos de Cloud Computing até a manipulação básica de containers e imagens.

## ☁️ 1. Conceitos de Cloud e Modernização

### Modelo Cliente-Servidor
A computação moderna evoluiu do modelo local para a nuvem para resolver altos custos de DataCenters físicos.
* **Baseado em requisições:** Servidores processam diferentes tipos de chamadas.
* **Escalabilidade:** Capacidade de lidar com grandes cargas de serviços de forma flexível.

### O Surgimento dos Containers
* **Microserviços:** Abordagem onde o software consiste em pequenos serviços independentes que se comunicam via API.
* **Virtualização vs. Containers:** Enquanto a virtualização entrega um Sistema Operacional completo (pesado), os containers isolam apenas a aplicação e suas dependências, compartilhando o Kernel do host (leve e rápido).

> **Definição:** Containers são unidades que reúnem um aplicativo e todos os arquivos necessários para sua execução, garantindo que ele funcione em qualquer SO ou ambiente.

---

## 🛠️ 2. Instalação (Ubuntu Server)

Para configurar o ambiente Docker de forma otimizada no Ubuntu, utilizamos o script oficial:

```bash
# Baixa o script de instalação oficial
curl -fsSL https://get.docker.com -o get-docker.sh

# Executa o script
sudo sh get-docker.sh
```

---

## 🚀 3. Primeiros Passos com Docker

O fluxo de trabalho baseia-se em imagens hospedadas no [Docker Hub](https://hub.docker.com/).

### 📦 Gestão de Imagens e Tags
As **Tags** definem a versão da imagem (ex: `debian:9` ou `mysql:latest`).

| Comando | Descrição |
| :--- | :--- |
| `docker pull <imagem>:<tag>` | Faz o download da imagem para a máquina local. |
| `docker images` | Lista todas as imagens disponíveis localmente. |
| `docker rmi <id_imagem>` | Remove uma imagem do sistema. |

### 🏃 Gestão de Containers
Abaixo, os comandos essenciais para o dia a dia:

* **Execução e Status:**
  * `docker run <imagem>`: Cria e inicia um container.
  * `docker run -d`: Roda o container em background (Detached).
  * `docker run -it`: Inicia de forma interativa (Terminal).
  * `docker ps`: Lista containers ativos.
  * `docker ps -a`: Lista todos os containers (ativos e encerrados).
  * `docker inspect <id>`: Exibe informações técnicas detalhadas do container.

* **Controle:**
  * `docker stop <nome/id>`: Para a execução.
  * `docker start <nome/id>`: Reinicia um container parado.
  * `docker rm <nome/id>`: Remove um container definitivamente.

* **Interação e Manipulação:**
  * `docker exec -it <nome> /bin/bash`: Entra no terminal de um container em execução.
  * `docker cp <origem> <destino>`: Copia arquivos entre a máquina local e o container (e vice-versa).

---

## 🗄️ 4. Laboratório Prático: MySQL

Exemplo de deploy de um banco de dados MySQL com mapeamento de portas e variáveis de ambiente:

```bash
docker run -d \
  --name mysql-A \
  -e MYSQL_ROOT_PASSWORD=Senha123 \
  -p 3306:3306 \
  mysql
```

**Parâmetros explicados:**
* `-d`: Modo background.
* `--name`: Nome personalizado para o container.
* `-e`: Define variável de ambiente (senha do root).
* `-p 3306:3306`: Mapeia a porta 3306 do host para a 3306 do container.

**Acesso ao banco:**
```bash
docker exec -it mysql-A bash
# Após entrar no container:
mysql -u root -p --protocol=tcp
```

---

## 💡 Dicas de Estudo

* **Sintaxe:** O Docker suporta a sintaxe nova (`docker container run`) e a antiga (`docker run`). A nova é recomendada para melhor organização.
* **Ajuda:** Utilize `docker --help` para consultar qualquer comando.
* **Persistência:** Containers são efêmeros. Lembre-se que ao remover um container, os dados internos são perdidos.

---
*Documentação criada como parte do curso Azure Advanced.*