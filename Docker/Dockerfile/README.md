# 📂 Módulo: Dockerfile - Automação e Imagens Personalizadas

Este módulo detalha o processo de criação de imagens customizadas, otimização com Multi-stage builds e a gestão de um servidor de imagens privado (Registry).

---

## 🏗️ 1. Entendendo o Dockerfile
O Dockerfile é o arquivo de "receita" que o Docker utiliza para construir uma imagem.

### Glossário de Comandos Novos:
* **FROM:** Define a imagem base.
* **RUN:** Executa comandos de terminal durante o build (instalação de pacotes).
* **COPY:** Copia arquivos locais para dentro do container.
* **ADD:** Similar ao COPY, mas extrai arquivos `.tar` automaticamente.
* **WORKDIR:** Define a pasta principal de trabalho (o "foco" do container).
* **LABEL:** Adiciona metadados (descrição/versão).
* **VOLUME:** Define um ponto de montagem para persistência.
* **EXPOSE:** Informa qual porta o container escuta.
* **ENTRYPOINT:** O comando que **não** pode ser sobrescrito facilmente ao iniciar.
* **CMD:** O comando padrão (ou argumentos) que pode ser alterado ao iniciar.

---

## 🐍 2. Lab: Python dentro de Ubuntu
Configurando um ambiente Python do zero em uma base Ubuntu.

**Dockerfile:**
```dockerfile
FROM ubuntu
RUN apt update && apt install -y python3 && apt clean
COPY app.py /apt/app.py
CMD ["python3", "/apt/app.py"]
```

**Comandos de Execução:**
```bash
docker build . -t ubuntu-python
docker run -ti --name meu-app ubuntu-python
```

---

## 🌐 3. Lab: Imagem Personalizada Apache (Debian)
Este laboratório demonstra a preparação de arquivos no host antes do build.

### Preparação no Host (Sua Máquina):
```bash
mkdir debian-apache && cd debian-apache/
mkdir site && cd site/
wget https://site1368633667.hospedagemdesites.ws/site1.zip
unzip site1.zip && rm site1.zip
tar -czf site.tar ./        # Compacta o site
cp site.tar ../ && cd ..    # Move para a raiz do projeto
rm -Rf site                 # Limpa a pasta temporária
```

**Dockerfile:**
```dockerfile
FROM debian
RUN apt-get update && apt-get install -y apache2 && apt-get clean

# O comando ADD extrai o .tar automaticamente para o destino
ADD site.tar /var/www/html

LABEL description="Apache Webserver 1.0"
VOLUME /var/www/html/
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachectl"]
CMD ["-D", "FOREGROUND"]
```

**Execução:**
```bash
docker image build -t debian-apache:1.0 .
docker run -dti -p 80:80 --name meu-apache debian-apache:1.0
```

---

## 🏗️ 4. Multi-Stage Build (Otimização Máxima)
O objetivo é reduzir drasticamente o tamanho da imagem final, separando o ambiente de compilação (Go) do ambiente de execução (Alpine).

**App Go (`app.go`):**
```go
package main
import ("fmt")

func main() {
  fmt.Println("Qual é o seu nome:? ")
  var name string
  fmt.Scanln(&name)
  fmt.Printf("Oi, %s! Eu sou a linguagem Go! ", name)
}
```

**Dockerfile Multi-Stage:**
```dockerfile
# Estágio 1: Compilação
FROM golang as execgo
COPY app.go /go/src/app/
ENV GO111MODULE=auto
WORKDIR /go/src/app/
RUN go build -o app.go .

# Estágio 2: Execução (Imagem leve)
FROM alpine
WORKDIR /appexecgo
COPY --from=execgo /go/src/app/app.go /appexec
RUN chmod -R 755 /appexec
ENTRYPOINT ["./appexec"]
```

---

## ☁️ 5. Distribuição: Docker Hub
1. `docker login` (Informe usuário e senha).
2. `docker build . -t seu_usuario/my-go-app:1.0`
3. `docker push seu_usuario/my-go-app:1.0`

---

## 🔒 6. Registry: Criando seu Próprio Servidor de Imagens

### 🖥️ NO SERVIDOR (Onde as imagens ficarão guardadas):
```bash
# Sobe o serviço de registro na porta 5000
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

### 💻 NA SUA MÁQUINA CLIENTE (Onde você desenvolve):

**1. Taguear a imagem para o servidor remoto:**
```bash
docker image tag [ID_DA_IMAGEM] [IP_DO_SERVIDOR]:5000/my-go-app:1.0
```

**2. Configurar o Docker para aceitar conexões HTTP (Insecure):**
Edite o arquivo: `nano /etc/docker/daemon.json`
```json
{
  "insecure-registries": ["IP_DO_SERVIDOR:5000"]
}
```

**3. Reiniciar o Docker para aplicar as mudanças:**
```bash
systemctl restart docker
```

**4. Subir a imagem para o seu servidor:**
```bash
docker push [IP_DO_SERVIDOR]:5000/my-go-app:1.0
```

**5. Comandos de Verificação e Limpeza:**
```bash
# Ver o catálogo de imagens no servidor via API
curl [IP_DO_SERVIDOR]:5000/v2/_catalog

# Remover imagem local e baixar do seu próprio servidor
docker rmi -f [ID_DA_IMAGEM]
docker pull [IP_DO_SERVIDOR]:5000/my-go-app:1.0
```

---
*Documentação detalhada para curso Azure Advanced.*