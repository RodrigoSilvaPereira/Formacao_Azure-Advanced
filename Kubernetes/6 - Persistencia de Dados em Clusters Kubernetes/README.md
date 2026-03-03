# 💾 Módulo: Persistência de Dados em Cluster Kubernetes

Neste módulo vamos aprender:

- Conceito de PersistentVolume (PV)
- Conceito de PersistentVolumeClaim (PVC)
- Persistência local
- Persistência no GCP
- Limitações do ReadWriteOnce
- Solução com NFS (ReadWriteMany)

---

# 🧠 Introdução: PV e PVC

O gerenciamento de armazenamento é diferente do gerenciamento de instâncias computacionais.

Para resolver isso, o Kubernetes criou duas APIs:

- PersistentVolume (PV)
- PersistentVolumeClaim (PVC)

---

## 📦 PersistentVolume (PV)

- Representa um volume físico no cluster
- Ciclo de vida independente do Pod
- Pode ser:
  - NFS
  - iSCSI
  - Disco de Cloud
  - hostPath (local)

Ele abstrai **como o armazenamento é provido**.

---

## 📄 PersistentVolumeClaim (PVC)

- É uma requisição de armazenamento
- Define:
  - Tamanho
  - Modo de acesso
- O Kubernetes vincula automaticamente PVC ↔ PV

Ele abstrai **como o armazenamento é consumido**.

---

# 🖥️ Criando um PV Local

Arquivo: `pv.yml`

```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /meubanco/
```

Aplicar:

```bash
kubectl apply -f .\pv.yml
```

```bash
kubectl get pv
```

---

# 📄 Criando o PVC

Arquivo: `pvc.yml`

```bash
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: local
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
```

Aplicar:

```bash
kubectl apply -f .\pvc.yml
```

---

# 🗄️ MySQL com Volume Persistente

Arquivo: `mysql-local.yml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: Senha123
        - name: MYSQL_DATABASE
          value: meubanco
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: local
          mountPath: /var/lib/mysql
      volumes:
      - name: local
        persistentVolumeClaim:
          claimName: local
```

Aplicar:

```bash
kubectl apply -f .\mysql-local.yml
```

Verificar:

```bash
kubectl get pods
kubectl get deploy
```

Entrar no Pod:

```bash
kubectl exec --tty --stdin nome_do_pod -- /bin/bash
```

Acessar MySQL:

```bash
mysql -u root -p
```

Senha:
```
Senha123
```

Criar registro:

```
use meubanco;

CREATE TABLE mensagens (
    id int,
    nome varchar(50),
    mensagem varchar(100)
);

insert into mensagens(id, nome, mensagem)
VALUES (2, 'Roberto', 'Esse cara sou eu');

select * from mensagens;
```

Agora delete:

```bash
kubectl delete deploy mysql
```

Suba novamente:

```bash
kubectl apply -f .\mysql-local.yml
```

Verifique novamente os dados.

✔ Os registros permanecem.  
Porque o volume é independente do Pod.

---

# ☁️ Persistência no GCP (GKE)

Documentação oficial:

https://cloud.google.com/kubernetes-engine/docs/concepts/persistent-volumes

Arquivo: `pvc-pod-demo.yaml`

```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-gcp
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
  storageClassName: standard-rwo

---

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: Senha123
        - name: MYSQL_DATABASE
          value: meubanco
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: pvc-gcp
          mountPath: /var/lib/mysql
      volumes:
      - name: pvc-gcp
        persistentVolumeClaim:
          claimName: pvc-gcp
```

Aplicar:

```bash
kubectl apply -f .\pvc-pod-demo.yaml
```

Testar dados → deletar deploy → recriar → verificar.

Os dados permanecem.

---

# ⚠️ Limitação do ReadWriteOnce

ReadWriteOnce permite:

✔ Leitura  
✔ Escrita  
❌ Apenas em um nó

Se criar múltiplas réplicas → somente um nó terá acesso.

Para múltiplos nós, precisamos de:

```
ReadWriteMany
```

---

# 🌐 Solução: NFS com Cloud Filestore (GCP)

Criar instância no Filestore.

Arquivo: `http-nfs.yml`

```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: fileserver-httpd
spec:
  capacity:
    storage: 50Gi
  accessModes:
    - ReadWriteMany
  nfs:
    path: /dados
    server: 10.14.147.218

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: fileserver-httpd
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  volumeName: fileserver-httpd
  resources:
    requests:
      storage: 50Gi

---

apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd
spec:
  replicas: 6
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - image: httpd:latest
        ports:
        - containerPort: 80
          name: httpd
        volumeMounts:
        - name: fileserver-httpd
          mountPath: /usr/local/apache2/htdocs/
      volumes:
      - name: fileserver-httpd
        persistentVolumeClaim:
          claimName: fileserver-httpd
```

Aplicar:

```bash
kubectl apply -f .\http-nfs.yml
```

Agora:

✔ Todas as réplicas compartilham o mesmo armazenamento  
✔ Alta disponibilidade  
✔ Arquitetura real de produção  

---

# 🏗️ Conclusão Arquitetural

Você aprendeu:

- Volume efêmero vs persistente
- PV e PVC
- hostPath (dev/local)
- StorageClass dinâmica (cloud)
- ReadWriteOnce vs ReadWriteMany
- NFS distribuído

Isso é base real de:

- Sistemas stateful
- Bancos de dados
- Sistemas distribuídos
- Arquitetura cloud-native

---

# 📚 Formação Kubernetes — Encerramento do Módulo Kubernetes

- Confira o Desafio final para entender como todos os conceitos aprendidos se aplicam.

---

Formação Azure Advanced — Módulo Kubernetes