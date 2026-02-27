
# 🌐 Módulo: Serviços de Acesso para Kubernetes Pods

Neste módulo vamos aprender:

- Como acessar Pods **sem usar LoadBalancer**
- Como funciona o **NodePort**
- Como executar comandos dentro de um Pod
- Port Forward
- Deploy + Service no mesmo YAML
- Conexão entre aplicação e banco

Todos os códigos estão na pasta:

```bash
/Codigos/
```

---

# 🎯 Acessando Pods sem LoadBalancer

Para acessar um Pod externamente sem utilizar LoadBalancer, utilizamos o tipo:

```
Service → NodePort
```

Ele expõe o serviço em uma porta fixa do nó (range padrão: 30000–32767).

---

# 📄 nodePort.yml

Arquivo responsável por gerar a porta de acesso externo.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-php-service
spec:
  type: NodePort
  selector:
    app: myapp-php
  ports:
    - port: 80
      targetPort: 80
      #nodePort: 30007
```

📌 Se não especificar `nodePort`, o Kubernetes gera automaticamente.

---

# 🐳 Build e Push da Imagem

Na pasta dos códigos:

```bash
docker build . -t usuario_dockerhub/myapp-php:1.0
```

```bash
docker push usuario_dockerhub/myapp-php:1.0
```

⚠️ Atenção ao nome do usuário do Docker Hub.

---

# ☁️ Utilizando Kubernetes no GCP

Conecte ao cluster do Google Kubernetes Engine.

```bash
kubectl get pods
```

Aplicar os arquivos:

```bash
kubectl apply -f .\pod.yml
```

```bash
kubectl apply -f .\nodePort.yml
```

Verificar serviço:

```bash
kubectl get services
```

Detalhar:

```bash
kubectl describe service myapp-php-service
```

Veja a porta gerada (ex: 30005).

Liberar firewall no GCP:

```bash
gcloud compute firewall-rules create my-app-php --allow tcp:PORTA_GERADA
```

Agora:

- Pegue o IP de qualquer nó
- Use IP:PORTA

Exemplo:

```
http://34.xxx.xxx.xxx:30005
```

---

# 🖥️ Utilizando Minikube

```bash
kubectl apply -f .\pod.yml
```

```bash
kubectl get pods
```

```bash
kubectl apply -f .\nodePort.yml
```

```bash
kubectl get service
```

```bash
kubectl describe service myapp-php-service
```

Gerar URL automaticamente:

```bash
minikube service myapp-php-service --url
```

---

# 🐧 Executando Aplicações Dentro do Pod

Entrando no container:

```bash
kubectl exec --stdin --tty myapp-php -- /bin/bash
```

Agora você está dentro do container Linux.

Use normalmente:

```
ls
ps
cat
vi
```

Isso é fundamental para debug.

---

# 📦 Deployment + Service no Mesmo YAML

Primeiro, limpar ambiente:

```bash
kubectl delete pod myapp-php
```

```bash
kubectl delete service myapp-php-service
```

Criar pasta:

```bash
mkdir app-3-kubernetes
```

Criar `app-deployment.yml`:

```bash
apiVersion: v1
kind: Pod
metadata:
  name: myapp-php
  labels:
    app: myapp-php
spec:
  containers:
  - name: myapp-php
    image: usuario_dockerhub/myapp-php:1.0
    ports:
    - containerPort: 80

---

apiVersion: v1
kind: Service
metadata:
  name: myapp-php-service
spec:
  type: NodePort
  selector:
    app: myapp-php
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30005
```

Aplicar:

```bash
kubectl apply -f .\app-deployment.yml
```

---

# 🔁 Encaminhamento de Portas (Port Forward)

Criar `mysql.yml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mysql-pod
  labels:
    name: mysql-pod
spec:
  containers:
    - name: mysql
      image: mysql:latest
      env:
        - name: MYSQL_DATABASE
          value: meuBanco
        - name: MYSQL_ROOT_PASSWORD
          value: Senha123
      ports:
        - containerPort: 3306
```

Aplicar:

```bash
kubectl apply -f .\mysql.yml
```

```bash
kubectl get pod
```

Encaminhar porta:

```bash
kubectl port-forward pod/mysql-pod 3306:3306
```

Agora você pode acessar localmente:

```
localhost:3306
```

---

# 🗄️ Criando Tabela

```sql
CREATE TABLE mensagens (
    id int,
    nome varchar(50),
    mensagem varchar(100)
);

INSERT INTO mensagens (id,nome,mensagem)
VALUES (1, 'Carlos da Silva', 'Hello World!!');

SELECT * FROM mensagens;
```

⚠️ Problema:  
Se o Pod cair → os dados somem.  
Porque não há volume persistente.

---

# 🔗 Criando Conexão com Banco

Códigos em:

```
/Codigos/Exemplo_2
```

## 📦 Banco

```bash
cd .\database\
```

```bash
docker build . -t usuario_hub/meubanco:1.0
```

```bash
docker push usuario_hub/meubanco:1.0
```

```bash
kubectl apply -f .\db-deployment.yml
```

```bash
kubectl get pods
```

```bash
kubectl get service
```

---

## ⚙️ Backend

```bash
cd .\backend\
```

```bash
docker build . -t usuario_hub/php:1.0
```

```bash
docker push usuario_hub/php:1.0
```

```bash
kubectl apply -f .\php-deployment.yml
```

```bash
kubectl get pod
```

```bash
kubectl get service
```

Liberar firewall:

```bash
gcloud compute firewall-rules create backend --allow tcp:30005
```

Ver nós:

```bash
kubectl get nodes -o wide
```

No frontend:

- Coloque IP do nó
- Porta NodePort

Execute o frontend localmente.

Sistema funcionando ponta a ponta.

---

# 🧠 Conceito Arquitetural Importante

- NodePort → expõe via porta do nó
- Port Forward → túnel temporário
- Service → camada de rede
- Sem Volume → dados voláteis
- Separação DB + Backend → arquitetura correta

---

# 📚 Anotações de Aula  
Formação Azure Advanced — Módulo Kubernetes
