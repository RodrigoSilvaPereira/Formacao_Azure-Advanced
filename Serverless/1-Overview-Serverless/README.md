# 🚀 Módulo: Overview de Serverless

---

## 🎯 O que vamos aprender

Neste módulo você irá entender:

- O que realmente significa **Serverless**
- Como funciona a abstração de infraestrutura na nuvem
- O modelo de cobrança *Pay-per-Use*
- Benefícios estratégicos e técnicos
- Limitações e desafios reais
- Quando utilizar (e quando evitar) Serverless
- Como essa abordagem se encaixa na arquitetura moderna em Cloud

---

## 🧠 O que é Serverless?

Apesar do nome, **Serverless não significa “sem servidor”**.

Sempre existem servidores. A diferença é que:

> Você não provisiona, não gerencia, não configura e não mantém a infraestrutura.

O provedor de nuvem (como a Microsoft Azure) assume totalmente:
- Provisionamento
- Escalabilidade
- Patching
- Atualizações
- Balanceamento de carga
- Alta disponibilidade

Você foca apenas no **código** e na **regra de negócio**.

---

## 🔎 Conceito Fundamental

**Server = Servidor**

No modelo tradicional:
- Você cria VM
- Configura sistema operacional
- Instala runtime
- Configura escalabilidade
- Gerencia custo fixo

No modelo Serverless:
- Você escreve o código
- Publica na nuvem
- Define gatilhos (triggers)
- Paga apenas quando executa

---

## ⚙️ Como funciona na prática

Você escreve uma função ou lógica de aplicação diretamente na nuvem.

Ela pode ser acionada por:

- Requisição HTTP
- Evento em fila
- Upload de arquivo
- Evento de banco de dados
- Timer
- Evento de mensageria

A execução acontece sob demanda.

Sem servidor provisionado manualmente.

Sem custo quando está ocioso.

---

## 💰 Modelo de cobrança — Pay-Per-Use

Um dos pilares do Serverless é o modelo:

> **Pay to Use (Pague pelo uso)**

Você paga por:
- Número de execuções
- Tempo de execução
- Consumo de memória
- Eventos processados

Isso elimina:
- Custo fixo de infraestrutura
- Recursos ociosos
- Superdimensionamento

É extremamente eficiente para:
- Workloads variáveis
- Processamentos esporádicos
- APIs com tráfego imprevisível
- Automação orientada a eventos

---

## 🚀 Principais Vantagens

### 🔹 Desenvolvimento acelerado
Sem precisar montar infraestrutura, o tempo de entrega reduz drasticamente.

Você sai do:
> “Provisionar e configurar”

Para:
> “Escrever e publicar”

---

### 🔹 Escalabilidade automática
O provedor escala automaticamente conforme demanda.

- 1 requisição → 1 instância
- 10.000 requisições → múltiplas instâncias

Sem intervenção manual.

---

### 🔹 Alta disponibilidade nativa
Os serviços já nascem distribuídos.

---

### 🔹 Foco no negócio
Você trabalha na lógica.
A nuvem cuida do restante.

---

## ⚠️ Desvantagens e Desafios

Serverless não é bala de prata. Entender suas limitações é maturidade técnica.

---

### 🔸 1. Dependência do provedor (Vendor Lock-in)

Cada cloud possui:
- APIs próprias
- Configurações específicas
- Modelos distintos

Migrar pode exigir reescrita.

Mitigação:
- Usar padrões abertos
- Evitar serviços muito proprietários quando possível

---

### 🔸 2. Cold Start

Se uma função fica ociosa por muito tempo:
- O ambiente é descarregado
- Na próxima execução há um pequeno atraso

Isso pode impactar:
- APIs sensíveis a latência
- Sistemas de tempo real

Soluções:
- Planos premium
- Instâncias pré-aquecidas
- Arquitetura híbrida

---

### 🔸 3. Monitoramento e Depuração

Em aplicações distribuídas orientadas a eventos:
- O fluxo pode ficar fragmentado
- O rastreamento exige ferramentas adequadas

Boas práticas:
- Centralizar logs
- Usar tracing distribuído
- Implementar correlação de eventos

---

### 🔸 4. Limites de execução

Funções Serverless possuem:
- Limite de tempo
- Limite de memória
- Limite de tamanho de payload

Não são ideais para:
- Processamentos longos
- Cargas extremamente pesadas
- Aplicações monolíticas tradicionais

---

## 🏗️ Quando usar Serverless

Ideal para:

- APIs REST leves
- Microsserviços
- Integrações entre sistemas
- Processamento de eventos
- ETL orientado a evento
- Webhooks
- Automação
- Backends de aplicações mobile

---

## 🚫 Quando evitar

Evite quando:

- A aplicação exige latência extremamente baixa e constante
- O processamento é contínuo 24/7 (VM pode ser mais barato)
- Alto controle de infraestrutura é necessário
- Workloads muito previsíveis e estáveis

---

## 🧩 Serverless na Arquitetura Moderna

Serverless é pilar de:

- Arquitetura orientada a eventos
- Microsserviços
- Sistemas distribuídos
- Aplicações escaláveis sob demanda
- Integrações desacopladas

Ele permite criar sistemas:

- Mais modulares
- Mais resilientes
- Mais escaláveis
- Com menor custo operacional

---

## 🏁 Encerramento do Módulo

Neste módulo você consolidou a base conceitual de Serverless:

- Entendeu que não é “sem servidor”, mas “sem gerenciar servidor”
- Compreendeu o modelo Pay-Per-Use
- Avaliou vantagens e limitações reais
- Identificou quando aplicar estrategicamente

---

**Formação:** Azure Advanced — Serverless