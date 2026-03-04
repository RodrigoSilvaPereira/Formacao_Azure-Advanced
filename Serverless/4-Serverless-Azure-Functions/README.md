# ⚡ Módulo: Serverless e Azure Functions

---

# 🎯 Objetivo do Módulo

Neste módulo você irá:

- Entender profundamente o modelo Serverless
- Compreender a arquitetura por trás do Azure Functions
- Aplicar Serverless em um cenário real (migração de backend MEAN)
- Criar um CRUD completo usando Azure Functions + MongoDB
- Consolidar o pensamento arquitetural orientado a eventos

Aqui saímos da teoria e entramos na prática real.

---

# 🔷 Overview — Serverless

Serverless é um modelo de desenvolvimento nativo em nuvem onde:

- O desenvolvedor escreve código
- O provedor gerencia infraestrutura
- A aplicação escala automaticamente
- O custo é baseado em execução

Importante:

> Serverless não significa ausência de servidores.  
> Significa ausência de gerenciamento de servidores.

O provedor de nuvem assume:

- Provisionamento
- Escalabilidade
- Patch de sistema
- Alta disponibilidade
- Monitoramento base

---

# 🏗️ Arquitetura Serverless

Serverless geralmente é aplicado em:

## 🔹 Microsserviços

Funções pequenas, independentes e desacopladas.

Cada função executa:
- Uma única responsabilidade
- Um evento específico
- Uma operação de negócio

---

## 🔹 Arquitetura Multi-Tier

- Camada de apresentação
- Camada de API (Functions)
- Camada de dados

Substituímos um backend monolítico por funções independentes.

---

# 🚀 Implantação da Arquitetura Serverless

Fluxo típico:

Cliente → API Gateway → Azure Function → Banco de Dados

Ou ainda:

Evento → Trigger → Azure Function → Processamento → Output

Tudo escalando automaticamente.

---

# ✅ Vantagens do Serverless

- Pay-per-use
- Escala automática
- Menor custo operacional
- Alta disponibilidade nativa
- Foco na lógica de negócio
- Time-to-market acelerado

---

# ⚡ Azure Functions

## 📌 O que é?

Serviço de computação serverless da Azure que permite executar pequenos trechos de código sob demanda, sem necessidade de provisionar infraestrutura.

Modelo baseado em:

- Triggers
- Bindings
- Eventos

---

# 🔐 Segurança no Azure Functions

Pode ser configurada através de:

- AuthLevel (Anonymous, Function, Admin)
- Azure AD
- OAuth 2.0
- Chaves de API
- API Management
- Managed Identity
- VNET Integration

Boas práticas:

- Nunca usar Anonymous em produção
- Usar Managed Identity para acesso a banco
- Proteger via API Gateway
- Monitorar com Application Insights

---

# 🧠 Linguagens Suportadas

- C#
- JavaScript
- F#
- Java
- TypeScript
- Python
- PowerShell

Preview:
- Bash
- PHP

---

# 🛠️ O que é possível fazer com Azure Functions?

- Criar APIs REST
- Processar filas
- Processar eventos IoT
- Automatizar tarefas
- ETL orientado a eventos
- Integração entre sistemas
- Webhooks
- Backend para aplicações mobile

---

# 📦 Templates Importantes

Azure Functions possui diversos templates prontos:

- HTTPTrigger
- TimerTrigger
- CosmosDBTrigger
- BlobTrigger
- QueueTrigger
- EventGridTrigger
- EventHubTrigger
- ServiceBusQueueTrigger
- ServiceBusTopicTrigger

Neste módulo utilizamos principalmente:

HTTPTrigger → Execução via requisição HTTP

---

# 🔄 Migração do Back-End Local (MEAN) para Serverless

## 📌 Contexto do Projeto

Projeto MEAN estruturado em:

- Pasta api (Node.js backend)
- Pasta front (Angular frontend)
- Banco MongoDB

Classe principal:

Funcionario:
- idFuncionario
- nomeFuncionario
- cargo
- numeroIdentificador

Objetivo:

Remover backend tradicional e substituí-lo por Azure Functions.

---

# 🛠️ Estrutura do Projeto Azure Functions

Após criação:

- Pasta por função
- function.json (configuração do trigger)
- index.js (lógica da função)
- shared/mongo.js (conexão com banco)
- local.settings.json (configurações locais)

---

# 🔌 Conexão com MongoDB

Arquivo: shared/mongo.js

```javascript
const { MongoClient } = require("mongodb");

const config = {
  url: "mongodb://localhost:27017/crud-serverless-mongodb",
  dbName: "crud-serverless-mongodb"
};

async function createConnection() {
  const connection = await MongoClient.connect(config.url, {
    useNewUrlParser: true
  });
  const db = connection.db(config.dbName);
  return { connection, db };
}

module.exports = createConnection;
```

---

# 🧩 CRUD com Azure Functions

Cada operação virou uma função independente:

- CreateFuncionario (POST)
- GetFuncionarios (GET)
- GetFuncionarioById (GET)
- UpdateFuncionario (PUT)
- DeleteFuncionario (DELETE)

Arquiteturalmente isso significa:

Cada endpoint é uma unidade isolada, escalável e independente.

---

# 🧠 Conceito Arquitetural Importante

Antes:
- Backend monolítico
- Múltiplos controllers
- Dependência centralizada

Depois:
- Funções desacopladas
- Escalabilidade individual
- Infra gerenciada

Você transformou um backend tradicional em uma arquitetura orientada a funções.

---

# 🖥️ Execução Local

Instalação do Azure Functions Core Tools:

```bash
npm install -g azure-functions-core-tools
```

Executar localmente:

```bash
func host start
```

Porta padrão:

http://localhost:7071

---

# 🌐 Integração com Front-End

Basta alterar a URI no Angular:

```typescript
uri = 'http://localhost:7071/api';
```

Sem backend Node tradicional.

Agora o backend é Serverless.

---

# 🏗️ Resultado Final

Você construiu:

- API REST
- Persistência MongoDB
- Backend Serverless
- Arquitetura desacoplada
- Escalabilidade automática

Tudo com:

- Menos infraestrutura
- Menos complexidade operacional
- Maior elasticidade

---

# 📌 Aprendizado Arquitetural

Você consolidou:

- Conceito de Serverless
- Azure Functions na prática
- Estrutura de projeto
- Segurança básica
- Execução local
- Integração frontend + functions
- Migração de backend tradicional

Isso é uma mudança de mentalidade.

Você deixou de pensar como desenvolvedor backend tradicional.

Agora pensa como engenheiro de arquitetura distribuída.

---

# 🏁 Encerramento do Módulo

Neste módulo você:

- Entendeu o modelo Serverless
- Aplicou Azure Functions em cenário real
- Migrou backend MEAN para arquitetura Serverless
- Criou um CRUD completo
- Consolidou arquitetura orientada a eventos

**Formação:** Azure Advanced — Serverless