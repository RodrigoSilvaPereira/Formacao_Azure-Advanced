# 💾 Módulo: Armazenamento de Dados e Persistência

Neste módulo, exploramos como o Docker lida com a persistência de dados. Por padrão, containers são **efêmeros** (tudo o que é criado dentro deles é perdido ao serem removidos). Para evitar a perda de dados críticos, utilizamos estratégias de armazenamento externo.

---

## 🏗️ 1. Tipos de Armazenamento (Mounts)

O Docker oferece três formas principais de persistir dados, cada uma com um caso de uso específico:

### A. Bind Mounts
Vincula um diretório ou arquivo específico da sua **máquina host** diretamente dentro do container.
* **Vantagem:** Total controle sobre o caminho do arquivo.
* **Uso comum:** Passar arquivos de configuração ou pastas de código-fonte para desenvolvimento em tempo real.
* **Sintaxe:** `-v /caminho/no/host:/caminho/no/container`

### B. Named Volumes (Volumes Nomeados)
Gerenciados inteiramente pelo Docker em uma área reservada do sistema (`/var/lib/docker/volumes`).
* **Vantagem:** Mais fácil de fazer backup e gerenciar via CLI do Docker. Recomendado para produção.
* **Uso comum:** Bancos de dados e logs.
* **Sintaxe:** `-v nome_do_volume:/caminho/no/container`

### C. Dockerfile Volumes
Definidos dentro do arquivo de receita (`Dockerfile`) através da instrução `VOLUME`. Eles criam volumes anônimos caso nenhum nome seja especificado no `docker run`.

---

## 🗄️ 2. Laboratório: Persistência com MySQL (Bind Mount)

Neste exemplo, garantimos que os dados do banco não sumam se o container for deletado.

```bash
# 1. Criar a estrutura de pastas no Host
mkdir -p /data/mysql-A

# 2. Subir o container mapeando o volume
# HOST: /data/mysql-A  |  CONTAINER: /var/lib/mysql
docker run -d \
  --name mysql-A \
  -e MYSQL_ROOT_PASSWORD=Senha123 \
  -p 3306:3306 \
  --volume=/data/mysql-A:/var/lib/mysql \
  mysql

# 3. Inspecionar para validar o mapeamento
docker inspect mysql-A | grep -A 10 "Mounts"
```
> **Dica:** Se você deletar o container `mysql-A` e criar um novo apontando para `/data/mysql-A`, todos os seus bancos e tabelas estarão lá!

---

## 🌐 3. Laboratório: Servidor Web Estático (Apache)

Usando Bind Mount para servir um site customizado.

```bash
# 1. Preparar pasta e arquivo
mkdir -p /data/apache-A
cd /data/apache-A

# 2. Criar o index.html
cat <<EOF > index.html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Apache OK</title>
</head>
<body>
    <h1>Servidor Apache funcionando 🚀</h1>
    <p>Se você está vendo esta página, o container está servindo corretamente via Bind Mount.</p>
</body>
</html>
EOF

# 3. Rodar o container Apache (httpd)
docker run -d \
  --name apache-A \
  -p 80:80 \
  --volume=/data/apache-A:/usr/local/apache2/htdocs \
  httpd
```

---

## 🐘 4. Laboratório: PHP + Apache (Site Dinâmico)

Para aplicações dinâmicas, utilizamos imagens que já possuem o runtime do PHP integrado ao Apache.

```bash
# 1. Criar pasta
mkdir -p /data/php-A
cd /data/php-A

# 2. Criar o arquivo PHP
cat <<EOF > index.php
<?php
echo "<h1>Apache + PHP funcionando</h1>";
echo "<p>Data e hora do servidor: " . date("d/m/Y H:i:s") . "</p>";
phpinfo();
?>
EOF

# 3. Rodar o container
docker run -d \
  --name php-A \
  -p 8080:80 \
  --volume=/data/php-A:/var/www/html \
  php:7.4-apache
```

---

## 🧹 5. Gerenciamento de Volumes

Comandos essenciais para manter o ambiente limpo:

| Comando | Descrição |
| :--- | :--- |
| `docker volume create data_mysql` | Cria um volume nomeado manual. |
| `docker volume ls` | Lista todos os volumes existentes. |
| `docker volume rm <nome>` | Remove um volume específico. |
| `docker volume prune` | Remove todos os volumes não utilizados. |

---
*Notas de aula: Azure Advanced - Módulo de Storage.*