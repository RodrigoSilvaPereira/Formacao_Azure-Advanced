# 📂 Módulo: Processamento, Logs e Redes

Este módulo foca na gestão avançada de recursos, diagnóstico de saúde dos containers e isolamento de tráfego através de redes personalizadas.

---

## ⚖️ 1. Limitação de Recursos (CPU e Memória)

Em ambientes de produção (como o AKS no Azure), nunca deixamos um container sem limites. Sem restrições, um container com erro de código pode consumir toda a RAM do host, derrubando outros serviços.

### Comandos de Gestão
| Comando | Descrição |
| :--- | :--- |
| `docker stats <nome>` | Dashboard em tempo real de CPU, RAM e Rede. |
| `docker update -m 128M --cpus 0.2 <nome>` | Altera os limites de um container em execução. |
| `docker run -m 128M --cpus 0.5` | Define limites máximos já na criação do container. |

### 🧪 Laboratório de Stress (Teste de Carga)
Podemos testar se os limites estão funcionando usando a ferramenta `stress`:

```bash
# 1. Criar container com limite de 128MB de RAM
docker run --name ubuntu-C -dti -m 128M --cpus 0.2 ubuntu

# 2. Instalar o stress dentro do container
docker exec -it ubuntu-C bash
apt update && apt install -y stress

# 3. Forçar o consumo de 50MB de RAM e 1 CPU
stress --cpu 1 --vm 1 --vm-bytes 50m
```
*Enquanto o comando acima roda, abra outro terminal e execute `docker stats ubuntu-C` para ver o Docker segurando o consumo conforme o limite imposto.*

---

## 🔍 2. Informações, Logs e Processos

Comandos essenciais para o "Troubleshooting" (resolução de problemas).

* **`docker info`**: Exibe o "estado da arte" do seu Docker Host. Mostra quantidade de containers (rodando/parados), versão do Kernel, driver de armazenamento e recursos totais do sistema.
* **`docker logs <nome>`**: O comando mais usado. Exibe tudo o que a aplicação enviou para o *STDOUT* (saída padrão). Vital para ver erros de inicialização.
* **`docker top <nome>`**: Mostra os processos que estão rodando **dentro** do container, incluindo o PID (Process ID) e o usuário.

---

## 🌐 3. Redes no Docker (Networking)

Por padrão, o Docker utiliza uma rede do tipo **Bridge**. No entanto, para maior segurança, criamos redes específicas para isolar diferentes partes da aplicação.

### Gestão de Redes
* **`docker network ls`**: Lista todas as redes disponíveis.
* **`docker network inspect bridge`**: Mostra quais containers estão conectados na rede padrão e seus respectivos IPs internos.
* **`docker network create minha-rede`**: Cria uma nova rede isolada.

### Prática: Isolamento de Container
```bash
# 1. Criar uma rede personalizada
docker network create rede-producao

# 2. Subir um container já conectado a essa rede
docker run -dti --name ubuntu-rede --network rede-producao ubuntu

# 3. Remover uma rede
docker network rm rede-producao
```

> **Dica de Cloud:** Containers na mesma rede personalizada podem se comunicar usando apenas o **nome do container** como endereço (DNS interno).

---
*Notas de aula: Azure Advanced - Módulo de Performance & Network.*