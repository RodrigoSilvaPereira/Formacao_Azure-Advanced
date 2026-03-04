
# 🌐 Módulo: Azure API Management Dashboard

---

## 🎯 Objetivo do Módulo

Neste módulo você vai entender como o **Azure API Management (APIM)** atua como camada estratégica entre consumidores e serviços backend, garantindo:

- Segurança
- Governança
- Observabilidade
- Controle de acesso
- Escalabilidade
- Monetização de APIs

Além disso, você verá como integrar API Management com **Azure Monitor, Log Analytics e Application Insights** para construir uma arquitetura verdadeiramente observável.

---

# 🔷 O Papel do Gerenciamento de API

O gerenciamento de API fornece funcionalidades fundamentais para garantir um programa de API bem-sucedido com base em:

- Envolvimento do desenvolvedor
- Insights de negócios
- Análise de consumo
- Segurança e proteção
- Governança

Cada API:

- Possui uma ou mais **operações**
- Pode ser associada a um ou mais **produtos**

Isso permite controlar quem acessa o quê, sob quais condições e com quais limites.

---

# 🧩 Componentes do Azure API Management

O sistema é composto por três principais componentes:

### 🔹 1. Gateway de API
Responsável por:
- Receber requisições
- Aplicar políticas
- Encaminhar para backend
- Coletar telemetria
- Aplicar segurança

É o componente que processa o tráfego em tempo real.

---

### 🔹 2. Portal do Azure
Interface administrativa para:
- Criar APIs
- Configurar políticas
- Monitorar uso
- Gerenciar produtos e grupos

---

### 🔹 3. Portal do Desenvolvedor
Portal público ou interno onde:
- Desenvolvedores descobrem APIs
- Consultam documentação
- Testam endpoints
- Obtêm chaves de assinatura

---

# 🏗️ Estrutura Organizacional no APIM

Dentro do API Management temos:

- **Produtos**
- **Grupos**
- **Desenvolvedores**
- **Políticas**
- **Portal do Desenvolvedor**

### Relação entre eles:

- Desenvolvedores pertencem a grupos
- Grupos têm acesso a produtos
- Produtos contêm APIs
- APIs possuem operações
- Políticas controlam comportamento

Essa estrutura é essencial para governança corporativa.

---

# 🚪 Função do Gateway de API

Um gateway de API fica entre clientes e serviços backend.

Ele atua como:

> Proxy reverso

Encaminhando solicitações do cliente para os serviços internos.

Além disso, ele pode:

- Aplicar autenticação
- Realizar terminação SSL
- Implementar limitação de taxa (rate limiting)
- Transformar requisições e respostas
- Coletar métricas
- Aplicar caching

---

# ⚠️ Problemas ao Implementar API Sem Gateway

Se você expõe serviços diretamente:

- Aumento da complexidade no código
- Cada cliente precisa conhecer múltiplos serviços
- Segurança espalhada em vários pontos
- Dificuldade de versionamento
- Falta de padronização
- Backend precisa expor protocolos amigáveis ao cliente

Isso viola princípios de desacoplamento.

---

# 🧠 Padrões de Design Funcional

## 🔹 1. Roteamento de Gateway

Uso do gateway como proxy reverso para rotear solicitações para um ou mais serviços backend.

Exemplo:
- /usuarios → serviço A
- /pedidos → serviço B

---

## 🔹 2. Agregação de Gateway

O gateway consolida múltiplas chamadas backend em uma única resposta.

Exemplo:
Um endpoint `/dashboard` pode:
- Chamar serviço de pedidos
- Chamar serviço de clientes
- Chamar serviço de pagamentos
- Retornar resposta consolidada

Reduz chamadas do cliente.

---

## 🔹 3. Descarregamento de Gateway (Offloading)

O gateway assume responsabilidades transversais:

- Autenticação
- SSL
- Logging
- Rate limit
- Cache
- Validação de token

O backend fica focado apenas na regra de negócio.

---

# 📜 O Papel das Políticas

Políticas são regras configuráveis aplicadas no pipeline da requisição.

Elas são organizadas em três estágios:

## 🔹 Inbound
Executadas antes da requisição chegar ao backend.

Exemplos:
- Validação de token
- Rate limit
- Transformação de cabeçalho
- Validação de IP

---

## 🔹 Backend
Executadas durante o encaminhamento ao serviço backend.

Exemplo:
- Alterar URL de destino
- Redirecionamento condicional

---

## 🔹 Outbound
Executadas antes da resposta ser enviada ao cliente.

Exemplos:
- Transformar JSON
- Remover campos sensíveis
- Adicionar cabeçalhos
- Caching de resposta

---

# ⚙️ Fluxo de Controle no Gateway

O APIM permite implementar fluxos como:

- Encaminhar solicitação
- Limite de simultaneidade
- Registrar no Event Hub
- Retornar resposta fictícia (mock)
- Repetir requisição em caso de falha

Isso permite criar camadas resilientes sem alterar backend.

---

# 🔐 Proteger APIs com Assinaturas

APIs podem exigir:

- Chave de assinatura
- Token JWT
- OAuth 2.0
- Certificado

Modelo mais comum:

> Subscription Key

Cada desenvolvedor recebe uma chave única.

---

## 🔹 Aplicativos que chamam APIs protegidas

Aplicações devem:

1. Obter chave no portal do desenvolvedor
2. Incluir no header:

```
Ocp-Apim-Subscription-Key: SUA_CHAVE
```

O gateway valida antes de encaminhar.

---

# 📊 Log Analytics

Um workspace do Log Analytics é o ambiente central de dados do **Azure Monitor**.

Ele armazena:

- Logs de aplicação
- Logs de infraestrutura
- Métricas
- Eventos

---

## Recursos Principais

- Insights
- Painéis
- Pastas de Trabalho (Workbooks)
- Análise de Logs (KQL)
- Alertas

---

## Retenção de Dados

- Retenção interativa (consulta rápida)
- Arquivamento de longo prazo

Isso permite equilíbrio entre custo e performance.

---

# 📈 Application Insights (Azure Monitor)

Serviço focado em monitoramento de aplicações.

## Recursos:

- Visibilidade completa
- Rastreamento distribuído
- Métricas de performance
- Análise de falhas
- Monitoramento de dependências
- Telemetria em tempo real

---

## Visualização no Application Insights

Você pode analisar:

- Tempo médio de resposta
- Taxa de falha
- Dependências externas
- Chamadas entre serviços
- Mapa de aplicação

Essencial para arquiteturas distribuídas.

---

# 🖥️ Azure Monitor

Plataforma unificada de monitoramento da Azure.

Permite monitorar:

- Aplicações
- Recursos Azure
- Assinaturas
- Locatários (tenants)

---

## Azure Monitor Agent (AMA)

O Azure Monitor Agent resolve a fragmentação de coleta de dados.

Ele:

- Substitui agentes legados
- Centraliza coleta
- Permite configuração baseada em regras

---

## Regras de Coleta de Dados (DCR)

Permitem definir:

- O que coletar
- De onde coletar
- Para onde enviar

Podem ser associadas a:

- Máquinas Virtuais
- Escalas de VM
- Serviços específicos

---

# 🔍 Observabilidade Avançada

## Exibição de Dependências (VM Insights)

Permite visualizar:

- Processos
- Conexões
- Dependências externas

---

## Topologia de Rede Virtual

Visualiza:

- Recursos conectados
- Fluxo de comunicação
- Componentes de rede

---

## Monitor de Conexão

Permite testar:

- Latência
- Disponibilidade
- Comunicação entre recursos

Muito útil para troubleshooting de rede.

---

# 🏁 Encerramento do Módulo

Neste módulo você compreendeu:

- O papel estratégico do API Management
- Como o gateway centraliza segurança e governança
- A importância das políticas no pipeline
- Como proteger APIs com assinatura
- Como integrar monitoramento com Azure Monitor
- Como utilizar Log Analytics e Application Insights
- Como estruturar uma arquitetura observável e resiliente

**Formação:** Azure Advanced — Serverless