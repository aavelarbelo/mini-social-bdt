# Arquitetura do Projeto

## Visão Geral

O projeto **Mini Social BDT** simula uma mini rede social utilizando três tecnologias NoSQL com papéis distintos:

- **MongoDB** para armazenamento dos dados principais da aplicação
- **Cassandra** para operações orientadas à consulta, como feed e notificações
- **Redis** para cache e acesso de baixa latência

O objetivo é distribuir responsabilidades entre tecnologias diferentes, usando cada uma de acordo com a sua principal vantagem.

---

## Fluxo Geral da Aplicação

```text
[Utilizador]
   │
   ├── cria publicação ───────────────→ [MongoDB]
   │
   ├── comenta / dá like ─────────────→ [MongoDB]
   │
   ├── abre feed ─────────────────────→ [Cassandra]
   │                                      │
   │                                      └── cache de feed recente → [Redis]
   │
   ├── consulta perfil ─────────────────→ [MongoDB]
   │                                      │
   │                                      └── cache de perfil → [Redis]
   │
   └── recebe notificações ─────────────→ [Cassandra]
                                          │
                                          └── notificações recentes → [Redis]
```

---

## Papel de Cada Tecnologia

### MongoDB

O MongoDB é responsável pelos **dados principais e documentais** da aplicação.

#### Usado para:
- utilizadores
- perfis
- publicações
- comentários
- likes

#### Motivo da escolha:
- modelo flexível baseado em documentos
- estrutura semelhante a JSON
- facilidade para representar entidades sociais com atributos variados
- suporte a índices para acelerar consultas

---

### Cassandra

O Cassandra é responsável pelos **dados orientados à consulta**, especialmente os que exigem leitura rápida e grande volume.

#### Usado para:
- feed por utilizador
- notificações por utilizador
- seguidores e seguindo (opcional, dependendo da modelação)

#### Motivo da escolha:
- alta escalabilidade
- excelente desempenho para consultas previsíveis
- modelação baseada nas queries
- uso explícito de **partition key** e **clustering columns**

---

### Redis

O Redis é responsável pelo **cache e pela redução de latência**.

#### Usado para:
- cache de perfil
- cache do feed recente
- notificações recentes
- contadores temporários
- dados com TTL

#### Motivo da escolha:
- acesso extremamente rápido em memória
- ideal para dados temporários
- suporte a TTL
- útil para reduzir leituras repetidas nas bases principais

---

## Estrutura Lógica dos Dados

### MongoDB
```text
users
posts
comments
```

### Cassandra
```text
feed_by_user
notifications_by_user
followers_by_user (opcional)
following_by_user (opcional)
```

### Redis
```text
profile:user:{id}
feed:user:{id}
notifications:user:{id}
```

---

## Exemplo de Distribuição das Responsabilidades

```text
MongoDB
- guarda o conteúdo principal da rede social

Cassandra
- organiza os dados para leitura rápida por utilizador

Redis
- guarda dados temporários e acelera acessos frequentes
```

---

## Testes e Métricas a Observar

Durante os testes do projeto, serão analisados os seguintes aspetos:

- **latência**: tempo de resposta das operações
- **throughput**: quantidade de operações processadas por período
- **comportamento do cache**: cache hit, cache miss, TTL e invalidação
- **resultados das queries**: consistência e desempenho das consultas

---

## Resumo da Arquitetura

```text
┌──────────────────────────────────────────────────────────────┐
│                       MINI SOCIAL BDT                       │
│        MongoDB + Cassandra + Redis em arquitetura NoSQL     │
└──────────────────────────────────────────────────────────────┘

[Utilizador]
   │
   ├── cria posts / comenta / dá like ─────→ MongoDB
   ├── consulta feed ───────────────────────→ Cassandra
   ├── consulta perfil ─────────────────────→ MongoDB
   └── acede a dados recentes ──────────────→ Redis

MongoDB   = dados principais
Cassandra = feed e notificações
Redis     = cache e baixa latência
```
