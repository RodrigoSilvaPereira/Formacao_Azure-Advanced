# 📂 Módulo: Docker Compose

O Docker Compose é uma ferramenta desenvolvida para ajudar a definir e compartilhar aplicativos com vários containers. Com o compose, você cria um arquivo **YAML** para definir os serviços e, com um único comando, pode rodar e parar todo o serviço.

> **Importante:** Sempre observe a versão do seu docker para garantir a compatibilidade.

## 🛠️ Instalação
No terminal do Ubuntu:
```bash
apt-get install -y docker-compose
docker-compose --help
```

---

## 🧪 1. Exemplo Prático: MySQL + Adminer

### Preparação do Ambiente:
```bash
cd /data/
mkdir mysql-C
cd ..
mkdir /compose
cd /compose
mkdir primeiro
cd primeiro/
nano docker-compose.yml
```

### Conteúdo do `docker-compose.yml`:
```yaml
version: '3.7'

services:
  mysqlsrv:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: "Senha123"
      MYSQL_DATABASE: "testedb"
    ports:
      - "3306:3306"
    volumes:
      - /data/mysql-C:/var/lib/mysql
    networks:
      - minha-rede

  adminer:
    image: adminer
    ports:
      - 8080:8080
    networks:
      - minha-rede

networks: 
  minha-rede:
    driver: bridge
```

### Execução:
```bash
docker-compose up -d
docker-compose down
```

---

## 🚀 2. Exemplo Avançado: PHP + Apache + MySQL

### Preparação do Arquivo PHP:
```bash
cd /data/
mkdir php/
cd php/
nano index.php
```

**Conteúdo do `index.php`:**
```php
<html>
<head><title>Exemplo PHP</title></head>
<?php
ini_set("display_errors", 1);
header('Content-Type: text/html; charset=iso-8859-1');

echo 'Versao Atual do PHP: ' . phpversion() . '<br>';

$servername = "db";
$username = "root";
$password = "Senha123";
$database = "testedb";

$link = new mysqli($servername, $username, $password, $database);

if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

$query = "SELECT * FROM tabela_exemplo";
if ($result = mysqli_query($link, $query)) {
    while ($row = mysqli_fetch_assoc($result)) {
        printf ("%s %s %s <br>", $row["nome"], $row["cidade"], $row["salario"]);
    }
    mysqli_free_result($result);
}
mysqli_close($link);
?>
</html>
```

### Configuração do phpMyAdmin:
```bash
mkdir admin
cd admin/
nano uploads.ini
```

### Montagem da Stack:
```bash
cd /data/
ls # Verifique se a pasta mysql-C existe
cd ..
cd /compose/
mkdir php-mysql
cd php-mysql
nano docker-compose.yml
```

**Conteúdo do `docker-compose.yml` final:**
```yaml
version: "3.7"

services:
  web:
    image: webdevops/php-apache:alpine-php7
    ports:
      - "4500:80"
    volumes:
      - /data/php/:/app
    networks:
      - minha-rede

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: "Senha123"
      MYSQL_DATABASE: "testedb"
    ports:
      - "3306:3306"
    volumes:
      - /data/mysql-C:/var/lib/mysql
    networks:
      - minha-rede

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      MYSQL_ROOT_PASSWORD: "Senha123"
    ports:
      - "8080:80"
    volumes:
      - /data/php/admin/uploads.ini:/usr/local/etc/php/conf.d/php-phpmyadmin.ini
    networks:
      - minha-rede

networks:
   minha-rede:
      driver: bridge
```

### Rodando a Aplicação:
```bash
docker-compose up
```

---
*Notas de aula: Azure Advanced - Finalização do Módulo Docker.*