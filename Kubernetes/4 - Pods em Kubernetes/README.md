# 📦 Módulo: Conceitos Básicos sobre Pods em Kubernetes

Este módulo cobre:

- O que é YAML
- Estrutura de um Pod
- Como aplicar no Minikube
- Introdução a Deployments
- Escalabilidade
- Exposição via Service

---

# 🧾 O que é YAML?

YAML (YAML Ain’t Markup Language) é uma linguagem de serialização de dados amplamente utilizada para arquivos de configuração.

No Kubernetes, praticamente todos os recursos são definidos via arquivos `.yml` ou `.yaml`.

## 📌 Características do YAML

- Usa indentação (estilo Python)
- Não permite TAB (apenas espaços)
- Não utiliza `{}`, `[]` ou tags de fechamento
- É altamente legível

⚠️ Indentação incorreta quebra o arquivo.

---

# 🧱 Estrutura Básica de um Pod

```yml
apiVersion: v1
kind: Pod
metadata:
  name: app-html
  labels:
    app: app-html
spec:
  containers:
  - name: app-html
    image: imagem/app-html:1.0
    ports:
    - containerPort: 80
```

---

# 📄 Exemplo Real: simple-pod.yml

Arquivo localizado na pasta `/Codigos`.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: app-html
  labels:
    app: app-html
spec:
  containers:
  - name: app-html
    image: nginx:latest
    ports:
    - containerPort: 80
```

## 🔍 Explicação

- apiVersion → versão da API
- kind → tipo do recurso
- metadata → informações do objeto
- spec → estado desejado
- containers → lista de containers
- image → imagem utilizada
- containerPort → porta interna

---

# 🚀 Implementando no Minikube

```bash
minikube start
```

```bash
minikube status
```

```bash
kubectl get services
```

```bash
kubectl get pods
```

Aplicando o YAML:

```bash
kubectl apply -f .\simple-pod.yml
```

Verificando:

```bash
kubectl get pods
```

Detalhes completos:

```bash
kubectl get pod -o wide
```

⚠️ O IP ainda não está exposto externamente.

Removendo o Pod:

```bash
kubectl delete pod app-html
```

---

# 📦 Criando um Deployment (com Réplicas)

Arquivo: `simple-deployment.yml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-html-deployment
  labels:
    app: app-html
spec:
  replicas: 3
  selector:
    matchLabels:
      app: app-html
  template:
    metadata:
      labels:
        app: app-html
    spec:
      containers:
      - name: app-html
        image: httpd:latest
        ports:
        - containerPort: 80
```

⚠️ Correção importante: é `containers:` (plural).

---

# 🚀 Subindo o Deployment

```bash
kubectl apply -f simple-deployment.yml
```

```bash
kubectl get pods
```

---

# 🔎 Mais Informações

```bash
kubectl get deployment
```

```bash
kubectl describe deployment app-html-deployment
```

Escalar para 10:

```bash
kubectl scale deployment app-html-deployment --replicas=10
```

Voltar para 3:

```bash
kubectl scale deployment app-html-deployment --replicas=3
```

---

# 🌍 Expondo com LoadBalancer

```bash
kubectl expose deployment app-html-deployment --type=LoadBalancer --name=app-html --port=80
```

```bash
kubectl get service
```

## ☁️ Em Nuvem

Será exibido um IP público automaticamente.

## 🖥️ No Minikube

```bash
minikube service --url app-html
```

---

# 🧠 Conceito Arquitetural

- Pod → Unidade básica
- Deployment → Controlador
- Service → Camada de rede
- LoadBalancer → Integração com infraestrutura

Em produção, não criamos Pods isolados.  
Sempre usamos Deployment.

---

# 📚 Anotações de Aula  
Formação Azure Advanced — Módulo Kubernetes
