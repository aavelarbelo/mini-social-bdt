# Mini Rede Social com MongoDB, Cassandra e Redis

Este projeto simula uma **mini rede social** utilizando três tecnologias NoSQL diferentes.  
O objetivo é demonstrar como cada base de dados pode ser usada de acordo com o seu ponto forte.

O sistema inclui funcionalidades básicas como:
- criação de utilizadores
- publicações
- comentários
- feed de publicações
- notificações

---

## Objetivo

O objetivo do projeto é construir uma arquitetura distribuída utilizando diferentes bases de dados NoSQL para resolver problemas específicos.

Cada tecnologia é utilizada para um tipo de necessidade:

- **MongoDB** para dados principais da aplicação
- **Cassandra** para consultas rápidas e feed de utilizadores
- **Redis** para cache e dados temporários

Assim é possível melhorar desempenho, escalabilidade e organização dos dados.

---

## Arquitetura de dados

A arquitetura divide as responsabilidades entre três sistemas de armazenamento.

```text
Utilizador
   │
   ├── cria posts / comentários ───────→ MongoDB
   │
   ├── consulta feed ──────────────────→ Cassandra
   │
   └── dados recentes / cache ─────────→ Redis
```

Cada base de dados é usada de acordo com o seu modelo e vantagens.

---

## Tecnologias e respetivo papel

### MongoDB
Responsável pelos **dados principais da aplicação**.

Usado para:
- utilizadores
- publicações
- comentários
- likes

Motivo:
- modelo baseado em documentos
- estrutura flexível
- fácil representação de entidades sociais

### Cassandra
Responsável pelo **feed e notificações dos utilizadores**.

Usado para:
- feed de publicações por utilizador
- notificações
- relações entre utilizadores (seguidores)

Motivo:
- alta escalabilidade
- excelente desempenho em consultas previsíveis
- modelo orientado à query

### Redis
Responsável pelo **cache e redução de latência**.

Usado para:
- cache de perfil
- cache do feed recente
- notificações recentes
- contadores temporários

Motivo:
- acesso extremamente rápido em memória
- suporte a TTL
- ideal para dados temporários

---

## Como executar

Para executar o ambiente do projeto será utilizado **Docker Compose**.

Passos básicos:

1. Clonar o repositório

```bash
git clone <repo-url>
```

2. Entrar na pasta do projeto

```bash
cd mini-social-bdt
```

3. Subir os serviços

```bash
docker compose up
```

Isso irá iniciar:
- MongoDB
- Cassandra
- Redis

---

## Fluxos obrigatórios

O sistema deverá suportar os seguintes fluxos principais:

1. **Criar utilizador**
2. **Criar publicação**
3. **Dar like ou comentar numa publicação**
4. **Gerar feed temporal de publicações**
5. **Consultar notificações**

Esses fluxos permitem demonstrar o uso combinado das três bases de dados.

### Mapeamento dos fluxos para as tecnologias

Cada fluxo utiliza a base de dados mais adequada ao tipo de operação.

- **Criar utilizador → MongoDB**

- **Criar publicação → MongoDB**

- **Feed temporal → Cassandra**

- **Notificações → Cassandra + Redis**

- **Cache de perfil e feed → Redis**
