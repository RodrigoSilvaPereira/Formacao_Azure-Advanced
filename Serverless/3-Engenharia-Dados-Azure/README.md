# 🏗️ Módulo: Engenharia de Dados na Azure

---

# 🎯 Objetivo do Módulo

Neste módulo você vai entender como a Azure suporta todo o ciclo de Engenharia de Dados:

- Ingestão
- Armazenamento
- Processamento
- Transformação
- Modelagem
- Entrega para consumo analítico

A ideia aqui não é apenas conhecer serviços, mas entender **como eles se conectam dentro de uma arquitetura moderna de dados**.

---

# 🔷 Ingestão de Dados

## 📌 O que é Ingestão de Dados?

É o processo de carregar ou importar dados de uma ou mais fontes para um serviço de armazenamento.

Fluxo conceitual:

Dados → Ingestão → Repositório (Data Lake / Banco / Data Warehouse)

Em ambientes de Big Data, a ingestão precisa ser:

- Rápida o suficiente para lidar com alto volume
- Escalável
- Resiliente
- Capaz de processar dados quase em tempo real (quando necessário)

---

# 🔧 Serviços de Ingestão na Azure

Principais serviços:

- Azure Data Factory
- PolyBase
- SQL Server Integration Services (SSIS)
- Azure Databricks

---

# 🚀 Azure Data Factory (ADF)

## 📌 O que é?

Serviço de orquestração, ingestão e transformação de dados.

Permite:

- Conectar a múltiplas fontes (on-premises e cloud)
- Orquestrar pipelines
- Executar ETL e ELT
- Integrar ambientes híbridos

---

## 🔁 O que é um Pipeline?

Um pipeline é:

> Um agrupamento lógico de atividades que juntas executam uma tarefa.

Cada atividade pode:

- Copiar dados
- Executar script
- Rodar Spark
- Chamar API
- Executar Data Flow

---

## 🔄 Fluxo de Dados (Data Flows)

São transformações visualmente projetadas dentro do Data Factory.

Permitem:

- Limpeza
- Join
- Agregações
- Derivações
- Filtros
- Conversões de schema

Sem necessidade de escrever código Spark manualmente.

---

## 🏗️ Criação de um Data Factory (Portal Azure)

Passos resumidos:

1. Acessar portal do Azure
2. Criar novo recurso
3. Escolher grupo de recursos
4. Definir região
5. Definir nome globalmente único
6. Selecionar versão V2
7. Criar
8. Abrir o **Azure Data Factory Studio**

O Studio é onde você:
- Cria pipelines
- Configura integrações
- Monitora execuções
- Gerencia datasets

---

## 🖥️ Criar via Azure CLI

Também é possível criar via linha de comando:

```
az datafactory create \
  --resource-group MeuGrupo \
  --factory-name MeuDataFactory \
  --location brazilsouth
```

Isso permite automação e integração com DevOps.

---

# 💾 Armazenamento de Dados

Uma conta de armazenamento Azure pode conter:

- Blobs
- Arquivos
- Filas
- Tabelas
- Discos

Características:

- Alta durabilidade
- Alta disponibilidade
- Segurança
- Escalabilidade global

---

## 🔐 Tipos de Redundância

### 🔹 LRS
Redundância local dentro de um datacenter.

### 🔹 ZRS
Redundância entre zonas na mesma região.

### 🔹 GRS
Replicação para outra região geográfica.

### 🔹 GZRS
Redundância por zona + replicação geográfica.

Escolha depende de:
- Criticidade
- SLA desejado
- Custo

---

# ⚙️ Processamento de Dados (ETL / ELT)

ETL:
Extract → Transform → Load

ELT:
Extract → Load → Transform

No mundo moderno (Data Lake), ELT é muito comum.

---

## 🔥 Azure Databricks

Baseado em Apache Spark.

Ideal para:

- Processamento paralelo
- Big Data
- Machine Learning
- Engenharia de dados distribuída

Permite:

- Jobs automatizados
- Pipelines escaláveis
- Integração com Data Lake
- Processamento massivo

---

# 🧠 Azure Synapse Analytics

Plataforma analítica unificada.

Suporta:

- Data Warehouse
- Big Data
- Spark
- SQL Serverless
- SQL Dedicated

Pode consultar dados usando:

- Transact-SQL
- Spark

---

## 🖥️ Synapse Studio

Interface unificada para:

- Criar pipelines
- Executar consultas SQL
- Rodar notebooks Spark
- Gerenciar modelos analíticos

Ele integra:

- Data Factory
- Spark Pools
- SQL Pools
- Data Lake

É um ambiente completo de engenharia + análise.

---

# 🔷 Azure HDInsight

Serviço baseado em clusters gerenciados de:

- Hadoop
- Spark
- Kafka
- Hive

Usado para:

- Processamento distribuído
- Transformações avançadas
- Streaming com Kafka

---

## 📌 Transformação vs Preparação

Transformação:
Converter formato ou estrutura dos dados.

Preparação:
Organizar, limpar e estruturar dados para análise.

---

# 🔄 Estágios do Processamento de Dados

## 1️⃣ Ingestão
Captura de dados brutos de múltiplas fontes.

## 2️⃣ Armazenar
Salvar dados em Data Lake ou armazenamento estruturado.

## 3️⃣ Preparar e Transformar
Limpeza, joins, agregações, normalizações.

## 4️⃣ Modelar e Fornecer
Criação de camadas:

- Curated
- Gold
- Data Marts
- Exposição para BI

---

# 🌊 Azure Stream Analytics

## 📌 O que é?

Serviço de processamento de dados em tempo real.

Permite:

- Processar eventos em streaming
- Analisar fluxos contínuos
- Detectar padrões
- Criar alertas

---

## 🔎 Casos de Uso

- IoT
- Telemetria
- Logs em tempo real
- Monitoramento de sensores
- Detecção de fraude
- Análise de tráfego

---

## ⚙️ Como funciona?

1. Entrada de dados:
   - Event Hub
   - IoT Hub
   - Blob Storage

2. Query usando linguagem similar a SQL:

```
SELECT
    deviceId,
    AVG(temperature) AS avgTemp
INTO
    output
FROM
    input TIMESTAMP BY eventTime
GROUP BY
    deviceId,
    TumblingWindow(minute, 5)
```

3. Saída para:
   - Power BI
   - SQL
   - Data Lake
   - Event Hub

---

## 🔥 Vantagens

- Totalmente gerenciado
- Escalável
- Baixa latência
- Fácil integração com IoT

---

# 🏁 Encerramento do Módulo

Neste módulo você compreendeu o ciclo completo da Engenharia de Dados na Azure:

- Como ingerir dados
- Como armazenar com redundância adequada
- Como processar com Spark e Databricks
- Como consultar com Synapse
- Como tratar dados em tempo real com Stream Analytics
- Como estruturar pipeline moderno de dados

**Formação:** Azure Advanced — Serverless