# Mini Social Network with MongoDB, Cassandra and Redis

This project simulates a **mini social network** using three different NoSQL technologies.  
The goal is to demonstrate how each database can be used according to its strengths.

> 🚧 **Status:** in development. The data modelling and the scripts for the three
> databases (schema, seed and queries for Cassandra, MongoDB and Redis) are
> implemented and documented, and the environment runs via Docker Compose. The
> application/API layer and the complete flows are in progress.

The system covers features such as:
- user creation
- posts
- comments
- post feed
- notifications

---

## Objective

The goal of the project is to build a distributed architecture using different NoSQL databases to solve specific problems.

Each technology is used for a particular need:
- **MongoDB** for the application's core data
- **Cassandra** for fast queries and user feeds
- **Redis** for caching and temporary data

This makes it possible to improve performance, scalability and data organization.

---

## Data architecture

The architecture splits responsibilities across three storage systems.

```text
User
   │
   ├── creates posts / comments ───────→ MongoDB
   │
   ├── queries feed ───────────────────→ Cassandra
   │
   └── recent data / cache ────────────→ Redis
```

Each database is used according to its model and advantages.

---

## Technologies and their roles

### MongoDB
Responsible for the **application's core data**.

Used for:
- users
- posts
- comments
- likes

Reason:
- document-based model
- flexible structure
- easy representation of social entities

### Cassandra
Responsible for the **user feed and notifications**.

Used for:
- per-user post feed
- notifications
- relationships between users (followers)

Reason:
- high scalability
- excellent performance on predictable queries
- query-oriented model

### Redis
Responsible for **caching and latency reduction**.

Used for:
- profile cache
- recent feed cache
- recent notifications
- temporary counters

Reason:
- extremely fast in-memory access
- TTL support
- ideal for temporary data

---

## How to run

The project environment runs with **Docker Compose**.

Basic steps:

1. Clone the repository
```bash
git clone <repo-url>
```

2. Enter the project folder
```bash
cd mini-social-bdt
```

3. Start the services
```bash
docker compose up
```

This will start:
- MongoDB
- Cassandra
- Redis

---

## Core flows

The system is intended to support the following main flows:

1. **Create user**
2. **Create post**
3. **Like or comment on a post**
4. **Generate a time-based post feed**
5. **Check notifications**

These flows demonstrate the combined use of the three databases.

### Mapping flows to technologies

Each flow uses the database best suited to the type of operation.

- **Create user → MongoDB**
- **Create post → MongoDB**
- **Time-based feed → Cassandra**
- **Notifications → Cassandra + Redis**
- **Profile and feed cache → Redis**

---

## Current status

- ✅ Modelling of the three databases, documented in `docs/`
- ✅ Schema, seed and query scripts (Cassandra `.cql`, MongoDB `.js`, Redis)
- ✅ Local orchestration with Docker Compose
- 🔄 Application/API layer and complete flows — in development

---

## 🌱 Next Evolution — a Data-Transition Community Platform (Planned)

This polyglot prototype (Cassandra + MongoDB + Redis, orchestrated with Docker
Compose) is the technical foundation for a larger idea.

**Context.** Moving into data from another field is a journey with great
potential and little structured guidance. Whatever the background — engineering,
management, healthcare, humanities — prior experience can accelerate the
transition when it is well channeled, rather than treated as something to leave
behind. This next evolution turns that observation into a product: a social
platform focused on people transitioning into a data career, where each
person's path becomes shared learning.

**Planned architecture** (reusing this prototype as the data layer):
- **MongoDB** — user profiles, posts and "learning journeys" (flexible documents).
- **Cassandra** — activity feeds/timelines (write-heavy, partitioned by user).
- **Redis** — sessions, caching and real-time counters (likes, notifications).
- **Backend API** — Python (FastAPI), containerized with Docker.
- **Local orchestration** — Docker Compose (already present in this repo).
- **Evolution** — cloud deployment (AWS) and CI/CD, areas I'm currently developing.

**Data & analytics layer** (my focus): anonymized engagement and learning-path
metrics, to understand what actually helps someone make the transition.

**Roadmap (planned):**
- [ ] Model social entities on top of the existing polyglot base
- [ ] Minimal API (authentication, profiles, posts, feed)
- [ ] Real-time counters and caching with Redis
- [ ] Community metrics dashboard
- [ ] Cloud deployment + CI/CD

**Principles:** honesty (sharing the real journey, mistakes included), community
(built with and for people in transition), continuous evolution (deepening and
citing the knowledge gained during the postgraduate), and focus on those in
transition.

> 🚧 Long-term direction, currently in conception. A separate project, developed
> incrementally.
