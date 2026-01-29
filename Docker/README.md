# Conhecendo e instalando o Docker

## Modelo Cliente-Servidor
 - Baseado em requisições
 - Servidores
 - Diferentes tipos de requisições
 - Grande carga de serviços (DataCenter ou Cloud)


 Solução para altos custos de servidores locais, surgiu a Cloud

# Cloud

- Virtualização
- Microserviços (Abordagem arquitetônica e organuzacional do desenvolvimento no qua o software consiste em pequenos serviços independentes que se comunicam via API)
- Abstração e escalabilidade
- Surge a tecnologia dos containers

# Containers

- Containers são uma tecnologia usada para reunir um aplicativo e todos os seus arquivos necesários em um ambiente de tempo de tempo de execuçã. Como uma unidade, o caontaner pode ser afacilmetne movi oe executado em qualquer SO, em qualquer contexto.

## O que é o Docker?

- Com o docker, é possível lidar com os contaienrs como se fossem máquinas virutais e extremamente leves. Além disso, os containers oferecem maior flexibilidade para você criar, implmentar, copiar e migrar um contaienr de um ambiente para o outro.
- Diferença de virtualização e containers

## Instalando o Docker (Ubuntu Server)

Como instalar o docker no Ubuntu

Site: https://docs.docker.com/engine/install/ubuntu/

Script oferecido:

curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
Executing docker install script, commit: 7cae5f8b0decc17d6571f9f52eb840fbc13b2737
<...>


Seguir passo a passo oferecido no tutorial para a versão desejada e a forma desejada

-----------------------------------------------------------

# Primeiros passos com docker

## Download de Imagens

- Acessar docker hub: https://hub.docker.com/
- Procurar a imagem que deseja (Exemplo: Hello World)
- Utilizar o comando par sua instalação:
docker pull hello-world:nanoserver-ltsc2025

### Principais comandos:
- Docker images (Listar imagens)
- Docker run NOME-DA-IMAGEM (Inicializar a imagem)
- Docker ps (Listar container em execução)
- Docker ps -a (tag -a, serve para listar containers em execução e que já executaram e cessaram)
- Docker run sleep 10 (explicar)
- Docker stop ID or NAME DO CONTAINER (parar contaienr)
- Docker run -it (explicar)
- Velha e nova sintaxe (docker e docker container)
- docker --help (ajuda)
- docker run -d (rodar em background)
- docker run -dti ubuntu
- docker exec -it xxx /bin/bash (exemplo)
- docker stop xxxx (parar contaienr)
- docker rm xxxx (excluir container)
- docker rmi (excluir imagem)
- docker exec Ubuntu-A mkdir /destino(criando pasta em contaienr docker)
- docker cp MeuArquivo.txt Ubuntu-A:/destino (enviando arquivo local para container)
- docker cp Ubuntu-A:/destino/Meuzip.zip Zipcopia.zip (enviando arquivo do container para local)
- docker stop NOME 
- docker start NOME
- docker inspect

### TAGS

- docker pull debian:9 (tag após o :,  com a versão desejada)
  
### Criando container do MySQL

- docker pull mysql
- docker run -e MYSQL_ROOT_PASSWORD=Senha123 --name mysql-A -d -p 3306:3306 mysql (explicar todas as partes)
- docker exec -it mysql-A bash
- mysql -u root -p --protocol=tcp (ele vai pedir a password, e você vai colocar a senha)

