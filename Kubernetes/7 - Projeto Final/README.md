
# 🚀 Desafio Final — Deploy de Aplicação Node.js no Kubernetes (GCP)

## 🎯 Objetivo

Criar um pipeline de deploy para uma aplicação Node.js utilizando:

- Docker
- Google Container Registry (ou Artifact Registry)
- Google Kubernetes Engine (GKE)
- CI/CD com estágios:
  - Realizar_testes
  - buildar_images
  - deploy-dev

Código base da aplicação:

```
https://gitlab.com/denilsonbonatti/gitlab-cicd-app-base/-/tree/main
```

---

# 🏗️ Arquitetura Final

Fluxo completo do deploy:

```
Código → Pipeline CI → Build Docker → Push Registry → Deploy GKE → Service → IP Público
```

Arquitetura no GCP:

```
GKE Cluster
 ├── Deployment (Node App)
 ├── ReplicaSet
 ├── Pods (2 réplicas)
 └── Service (LoadBalancer)
```

---

# ☁️ 1️⃣ Criando o Cluster no GCP

Acesse:

```
Google Cloud Console → Kubernetes Engine → Create Cluster
```

Configurações recomendadas:

- 2 nodes
- Machine type padrão (e2-medium)
- Região próxima

Após criar o cluster, conectar via CLI:

\`\`\`bash
gcloud container clusters get-credentials NOME_CLUSTER --zone ZONA
\`\`\`

Verificar conexão:

\`\`\`bash
kubectl get nodes
\`\`\`

---

# 🐳 2️⃣ Criando o Dockerfile

\`\`\`bash
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install --production

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
\`\`\`

---

# 📦 3️⃣ Build e Push da Imagem

Build:

\`\`\`bash
docker build -t usuario_gcp/node-app:1.0 .
\`\`\`

Push:

\`\`\`bash
docker push usuario_gcp/node-app:1.0
\`\`\`

(Alternativamente usar Artifact Registry)

---

# 📄 4️⃣ Criando Deployment

Arquivo: `deployment.yml`

\`\`\`bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        image: usuario_gcp/node-app:1.0
        ports:
        - containerPort: 3000
\`\`\`

Aplicar:

\`\`\`bash
kubectl apply -f deployment.yml
\`\`\`

Verificar:

\`\`\`bash
kubectl get pods
kubectl get deploy
\`\`\`

---

# 🌐 5️⃣ Criando Service (LoadBalancer)

Arquivo: `service.yml`

\`\`\`bash
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  type: LoadBalancer
  selector:
    app: node-app
  ports:
    - port: 80
      targetPort: 3000
\`\`\`

Aplicar:

\`\`\`bash
kubectl apply -f service.yml
\`\`\`

Verificar IP público:

\`\`\`bash
kubectl get service
\`\`\`

Quando o `EXTERNAL-IP` aparecer, acessar:

```
http://IP_PUBLICO
```

---

# 🔁 6️⃣ Pipeline CI/CD

Arquivo `.gitlab-ci.yml`

\`\`\`bash
stages:
  - Realizar_testes
  - buildar_images
  - deploy-dev

variables:
  IMAGE_NAME: usuario_gcp/node-app

Realizar_testes:
  stage: Realizar_testes
  image: node:18
  script:
    - npm install
    - npm test

buildar_images:
  stage: buildar_images
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $IMAGE_NAME:$CI_COMMIT_SHORT_SHA .
    - docker push $IMAGE_NAME:$CI_COMMIT_SHORT_SHA

deploy-dev:
  stage: deploy-dev
  image: google/cloud-sdk:latest
  script:
    - echo "$GCP_SERVICE_KEY" > key.json
    - gcloud auth activate-service-account --key-file=key.json
    - gcloud container clusters get-credentials NOME_CLUSTER --zone ZONA
    - kubectl set image deployment/node-app node-app=$IMAGE_NAME:$CI_COMMIT_SHORT_SHA
\`\`\`

---

# 🔐 Variáveis Necessárias no GitLab

Configurar nas CI/CD Variables:

- `GCP_SERVICE_KEY`
- `CI_REGISTRY_USER`
- `CI_REGISTRY_PASSWORD`

---

# 📊 Validações Pós Deploy

\`\`\`bash
kubectl get pods
kubectl get svc
kubectl describe deployment node-app
\`\`\`

Testar escalabilidade:

\`\`\`bash
kubectl scale deployment node-app --replicas=5
\`\`\`

---

# 🧠 Conceitos Aplicados

- Containerização com Docker
- Registry
- Deployment declarativo
- Service LoadBalancer
- Pipeline CI/CD
- Atualização contínua via `kubectl set image`
- Escalabilidade horizontal

---

# 🏁 Resultado Final

✔ Aplicação Node.js containerizada  
✔ Pipeline automatizado  
✔ Deploy contínuo no GKE  
✔ Exposição pública via LoadBalancer  
✔ Escalável sob demanda  

---

# 📚 Formação Azure Advanced — Módulo Kubernetes - Desafio Final

Este projeto demonstra domínio prático de:

- Docker
- Kubernetes
- GCP
- CI/CD
- Deploy automatizado em nuvem

Arquitetura pronta para ambientes reais.